---
layout: post
title: Pass C Structure to Lua.
---

Often, an individual will be trying to learn a new programming language, or a new feature of a programming language, and will find it extremely difficult to find information on their specific topic.

Or there will be information out there, but it will be complete, and you'll have questions afterwards. I know I've come across this a lot. Just trying to fill a few of these gaps 

One such enigma for me have been Lua to C/C++ bindings. In theory, there's all the information there, but I had trouble gluing it all together.

This blog post is just my findings, not necessarily the best way, but it is _a_ way, and I understand it. 

First thing's first, you need to understand the concept of the stack, as that's how your C/Cpp will interact with Lua in the first place.

The general C convention for functions that get called from Lua is: 

```Cpp
//I prefix my function definitions with l_ in order to indicate this function takes a (lua_State *) and should be called from within Lua.
int l_myCFunctionThatGetsCalledFromLua(lua_State * L) {
  constexpr int numberOfReturnValues = 0;
  
  //DoStuff with Lua here.
  
  return numberOfReturnValues;
}
```

Let's say we have a C struct. How do we give this structure to Lua?

```Cpp
struct PlayerLocation {
   float x;
   float y;
   float z;
};

#define MAX_PLAYERS 4

static PlayerLocation playerLocations[MAX_PLAYERS];
```

Side note: My personal Lua is compiled to use single-precision 32-bit floats, because the target platform (Halo Custom Edition) is only 32 bit, and the game's vast majority floating point numbers are 
single-precision. 

Need something here, before we cover passing C-structure into Lua. 

First, the helper function: 
```Cpp
//where idx is the index on Lua's stack.
static auto getLuaInt(lua_State *L, int idx = 1) {
	if (!lua_isinteger(L, idx)) {
		lua_pushliteral(L, "incorrect argument! needed integer!");
		lua_error(L);
		return reinterpret_cast<uintptr_t>(-1);
	}

//default to uintptr_t, probably because of some bad habit of mine. Feel free to remove and default to a regular integer.
	return reinterpret_cast<uintptr_t>(lua_tointeger(L, idx));
}
```

Now, for how we pass this structure to lua:


```Cpp
//When in Lua, the user will only have to call GetPlayerLocation(PlayerIndex) b/c of the way we register functions in Lua
int l_GetPlayerLocation(lua_State * L) {
    // The first item on the stack to us should be the index of the player location.
    //Lua defaults to 1-indexing, but really programmers can use whatever index they want, so we're going to assume 0-indexed.
  
    unsigned int playerIdx = getLuaInt(L);
    int result = 0;
    //Since playerIdx is unsigned, we only need to check the max bounds. 
    if(playerIdx > MAX_PLAYERS - 1) { 
        lua_pushinteger(L, -1);
        return 1;
    }

  //Tell Lua we will be creating a table with 3 entries.  
  lua_createtable(L, 0, 3);
  
  auto currentPlayer = playerLocations[playerIdx];
  
  //Lua's default type is a "number" which is, in our case, a float. 
  lua_pushnumber(L, currentPlayer.x);
  lua_setfield(L, -2, "x");
   
  lua_pushnumber(L, currentPlayer.y);
  lua_setfield(L, -2, "y");
     
  lua_pushnumber(L, currentPlayer.y);
  lua_setfield(L, -2, "z");
 
  return 1;
}
```

In order to call this function in Lua: 
```Lua
function PlayerInBoundingVolume(playerIndex)
  local playerCoordinates = GetPlayerLocation(playerIndex)
  
  if playerCoordinates ~= -1 then   
     local x = playerCoordinates.x
     local y = playerCoordinates.y
     local z = playerCoordinates.z
     --Do stuff
 end
end
```

This code may have a syntax error, or an explanation may be incomplete. This isn't the prettiest, or even the most concise series of examples, but I hope you find it useful anyway.

