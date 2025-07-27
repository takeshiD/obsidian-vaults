# LSP Capabilities
- Diagnostics
- Inlay Hints
- Hover
- References
- Goto Definition
- Signature Help
- Semantic Token
- Completion

# Inlay Hints
## Assign Literal
```lua
-- No Display
local a = 1
local b = "hello"
```

## Assign Expression
```lua
local x = 12
local y = "hello"
local a: number = x
local b: string = y
```

## Assign Anonymous Function
```lua
---@param x number
---@param y number
---@return number
local f = function(x: number, y: number)
	return x + y
end
local result: number = f(x: 12, y: 34)
```