# Manual de lua con comentarios en español
## Indice
- [Variables](#variables)
- [Lógica booleana](#lógica-booleana)
- [Operaciones](#operaciones)
- [Concatenación de Cadenas](#concatenación-de-cadenas)
- [Patrones en Lua](#patrones-en-lua)
- [Control de flujo](#control-de-flujo)
- [Funciones](#funciones)
- [Constructores de Tablas](#constructores-de-tablas)
- [Ordenación de Tablas](#ordenación-de-tablas)
- [Entorno Global](#entorno-global)
- [Iteración con ipairs y pairs](#iteración-con-ipairs-y-pairs)





## Variables
```Lua
-- Lua es de tipado dinámico. Las variables no tienen tipos; solo los valores tienen tipos.
apple = 4    -- número
apple = true -- booleano
apple = nil  -- nil representa un valor nulo o inexistente

banana = "yellow"   -- cadena de texto
cherry = 'red'      -- las comillas simples también son válidas

print(banana)       -- imprime "yellow"
print(type(banana)) -- imprime "string"

d, e, f = "hi", false, 123 -- asignación múltiple

i = 10      -- variable global
local j = 1 -- variable local

local k = 42; -- los puntos y comas son mayormente opcionales

--[[
    Esto es un
    comentario multilínea
--]]

-- alcance de las variables https://www.lua.org/pil/4.2.html

if true then
    local x = 4
    print(x) -- 4
end

print(x) -- nil (fuera de alcance)
local y = 10
local z

-- un bloque do-end puede ser útil para definir explícitamente el alcance de las variables
do
    print(y) -- 10
    local y = 3
    print(y) -- 3
    z = true
end

print(y) -- 10
print(z) -- verdadero

```
## Lógica booleana


```lua
local apple, berry = 3, 5
print(apple == berry) -- falso
print(apple ~= berry) -- verdadero
print(apple <= berry) -- verdadero

-- ten cuidado al comparar valores de diferentes tipos
print(0 == "0")           -- falso
print(2 == tonumber("2")) -- verdadero
print("2" == tostring(2)) -- verdadero
-- solo nil y false son falsos, cualquier otra cosa es verdadera
local a = 123

if a then
    print("Esto es verdadero") -- imprime esto
else
    print("Esto es falso")
end

print(not nil)   -- verdadero
print(not false) -- verdadero

print(not true)  -- falso
print(not 0)     -- falso
print(not "")    -- falso
## Operadores Lógicos
-- operador and: si el primer operando es verdadero, entonces se devuelve el segundo operando, de lo contrario devuelve el primer operando
-- operador or: si el primer operando es verdadero, entonces se devuelve el primer operando, de lo contrario devuelve el segundo operando
print(true and 7)   -- primer op es verdadero -> devuelve el segundo op: 7
print(4 and 5)      -- primer op es verdadero -> devuelve el segundo op: 5
print(false and 13) -- primer op es falso -> devuelve el primer op: falso
print(nil and 9)    -- primer op es falso -> devuelve el primer op: nil

print(true or 7)    -- primer op es verdadero -> devuelve el primer op: verdadero
print(4 or 5)       -- primer op es verdadero -> devuelve el primer op: 4
print(false or 13)  -- primer op es falso -> devuelve el segundo op: 13
print(nil or 9)     -- primer op es falso -> devuelve el segundo op: 9

print(true and true and false) -- falso (no todos los operandos son verdaderos)
print(false or false or true)  -- verdadero (al menos un operando es verdadero)

-- tanto "and" como "or" utilizan evaluación de cortocircuito, es decir, evalúan su segundo operando solo cuando es necesario
local function greet() print("hola") end

local a = true and greet()  -- hola
local b = false and greet() -- no imprime nada

local c = true or greet()   -- no imprime nada
local d = false or greet()  -- hola
-- alterna entre verdadero/falso
local x = true

x = not x
print(x) -- falso
x = not x
print(x) -- verdadero
-- combinar los operadores "and" y "or" es similar al operador ternario
-- http://lua-users.org/wiki/TernaryOperator
local b = true and "sí" or "no"
local c = false and "sí" or "no"

print(b) -- sí
print(c) -- no
```
## Operaciones
```lua
-- se sigue el orden normal de operaciones
print(2 + 3 * 4)   -- 14
print((2 + 3) * 4) -- 20
-- Lua puede convertir automáticamente entre cadenas y números (coerción)
-- https://www.lua.org/manual/5.1/manual.html#2.2.1
local a = 3
local b = "2"

print(a + b) -- 5
print(math.max(23, 17)) -- 23
print(math.floor(2.9)) -- 2
print(math.cos(2 * math.pi)) -- 1

print(math.pow(2, 10)) -- 1024
print(2^10)            -- 1024

print(math.fmod(14, 3)) -- 2
print(14%3)             -- 2
-- WoW incluye una biblioteca de operaciones a nivel de bits
-- https://wow.gamepedia.com/Lua_functions#Bit_Functions
print(bit.band(0x10A48, 0x800)) -- 2048 (0x800)
print(bit.bor(1, 2)) -- 3
print(bit.lshift(1, 10)) -- 1024
```
## Concatenación de Cadenas
```lua
-- concatenación de cadenas https://www.lua.org/pil/3.4.html
print("Storm".."wind") -- Stormwind
print(40 .." yards")   -- 40 yards

local a, b = "Where", "wife"
print(a.." is Mankrik's "..b.."?") -- ¿Dónde está la esposa de Mankrik?

-- longitud de cadena https://www.lua.org/manual/5.1/manual.html#2.5.5
print(#a) -- 5

-- formatos de cadenas https://www.lua.org/manual/5.1/manual.html#pdf-string.format
print(string.format("Hola %s", "Bob")) -- Hola Bob

-- estilo orientado a objetos
local fs = "WTS %d Shiny %s"
print(fs:format(6, "Red Apples")) -- WTS 6 Shiny Red Apples

local s = "Hello my friend"
print(string.sub(s, 7))         -- mi amigo
print(string.gsub(s, "l", "w")) -- Hewwo my friend, 2
print(s:upper())                -- HELLO MY FRIEND
```

## Patrones en Lua
```lua
-- Lua utiliza "patrones" en lugar de expresiones regulares
print(string.match("abcdefg", "b..")) -- bcd (. coincide con todos los caracteres)
print(string.find("abcdefg", "b.."))  -- 2, 4

print(string.match("foo 123 bar", "%d%d%d")) -- 123 (%d coincide con un dígito)
print(string.match("hello World", "%u")) -- W (%u coincide con una letra mayúscula)

-- un patrón puede contener sub-patrones encerrados en paréntesis, también conocidos como capturas
local m, d, y = string.match("04/19/64", "(%d+)/(%d+)/(%d+)")
print(m, d, y) -- 04, 19, 64
```

## Control de flujo
```lua
local x = math.random(1, 6)

if x == 3 then
    print(x, "es tres")
elseif x < 3 then
    print(x, "es menor que tres")
else
    print(x, "es mayor que tres")
end
## Bucle for
local sum = 0

for i = 1, 10 do
    sum = sum + i
end

print(sum) -- 55
## Bucle while
local sum, i = 0, 0

while true do
    i = i + 1
    sum = sum + i
    if i == 10 then
        break
    end
end

print(sum) -- 55
```

## Funciones
```lua
function square(v)
    return v * v
end

print(square(4)) -- 16
-- varargs puede contener un número variable de argumentos
-- https://www.lua.org/pil/5.2.html
local function add(...)
    print(...) -- 3, 5, 4
    local a, b, c = ...
    return a + b + c
end

-- devuelve la suma de 3 valores
print(add(3, 5, 4)) -- 12
-- la función select() devuelve todos los parámetros después del índice n-ésimo
-- https://www.lua.org/manual/5.1/manual.html#pdf-select
local function foo(...)
    print(select(2, ...)) -- verde, azul
end
foo("rojo", "verde", "azul")

local function bar()
    return "hola", "mundo", "adiós"
end
print(select(2, bar())) -- mundo, adiós

print(select(2, "a", "b", "c", "d")) -- b, c, d
print(select(3, "a", "b", "c", "d")) -- c, d
print(select(4, "a", "b", "c", "d")) -- d
-- un par extra de paréntesis captura el primer valor
print((select(2, "a", "b", "c", "d"))) -- b

Alpha = function() end
-- es lo mismo que
function Alpha() end
local Object = {}

Object.Bravo = function(Object) print(Object) end
-- es lo mismo que
function Object.Bravo(Object) print(Object) end
-- es lo mismo que
function Object:Bravo() print(self) end

-- y para llamar
Object.Bravo(Object)
-- es lo mismo que
Object:Bravo()
```
## Constructores de Tablas
```lua
-- los constructores de tablas pueden contener una lista separada por comas de objetos para crear un "array"
-- los índices de las tablas comienzan en uno en lugar de cero
local t = {"a", "b", "c"}
print(t[1]) -- a
print(t[3]) -- c

-- esto es azúcar sintáctico para
local t = {[1]="a", [2]="b", [3]="c"}
print(t[1]) -- a
print(t[3]) -- c

-- las tablas pueden anidarse
local t = {"test", {17, 43}}
print(t[2][1]) -- 17
print(t[2][2]) -- 43

-- tabla con pares clave-valor
local t = {["fruit"] = "peach", foo = true}
print(t.fruit)  -- peach
print(t["foo"]) -- true
print(t.foo)    -- true (azúcar sintáctico)
-- tabla utilizada como una lista / array ordenado secuencialmente
local t = {8, 5, 7}

print(t[1]) -- 8
t[2] = 14   -- cambia el valor en el índice 2
table.insert(t, "banana")

-- itera a través de la tabla con un bucle for
for i = 1, #t do
    print(i, t[i])
end
-- 1, 8
-- 2, 14
-- 3, 7
-- 4, banana

-- nota: esto es table.unpack() en Lua 5.2
print(unpack(t)) -- 8, 14, 7, banana
-- tabla utilizada como un diccionario / array asociativo
local t = {
    foo = 2,
    bar = 5,
    baz = 7,
}

print(t.bar) -- 5
t.fruit = "banana"

-- itera a través de la tabla con un bucle for genérico (orden aleatorio)
-- https://www.lua.org/pil/4.3.5.html
for k, v in pairs(t) do
    print(k, v)
end
-- baz, 7
-- bar, 5
-- fruit, banana
-- foo, 2
-- las tablas se pasan como una referencia https://stackoverflow.com/a/6128322
local a = {"strawberry", 12, true}
print(a) -- tabla: 0000022C0973C2A0

local b = a -- b apunta a la tabla en a
print(b == a) -- verdadero

-- cambiar una clave/valor en b también lo cambia en a ya que apuntan a la misma tabla
b[1] = "banana"
print(a[1]) -- banana

a = nil -- a ya no apunta a la tabla
print(a) -- nil
print(b) -- tabla: 0000022C0973C2A0 (b aún apunta a la tabla)

local c = {}
for key, value in pairs(b) do -- copias superficiales de una tabla
    c[key] = value
end
-- los contenidos de c son iguales a b pero son tablas diferentes en memoria
print(c == b) -- falso
-- consejo: puedes concatenar variables al indexar una tabla
local names = {
    party1 = "Bob",
    party2 = "Alice",
    party3 = "Elroy",
    party4 = "Flintlocke",
}

for i = 1, 4 do
    print(names["party"..i])
end
-- Bob
-- Alice
-- Elroy
-- Flintlocke
-- consejo: usa una tabla de búsqueda si quieres verificar si una tabla (muy grande) contiene un valor específico
local mounts = {352, 435, 464, 522}

local function contains(t, id)
    -- usamos el guión bajo _ como una variable ficticia cuando no nos interesa
    -- https://stackoverflow.com/questions/5893163
    for _, v in pairs(t) do
        if v == id then
            return true
        end
    end
end

print(contains(mounts, 464))
-- esto es mucho más rápido
local mounts = {
    [352] = true,
    [435] = true,
    [464] = true,
    [522] = true,
}

print(mounts[464])
```

## Ordenación de Tablas
```lua
-- las tablas secuenciales pueden ordenarse
-- http://lua-users.org/wiki/TableLibraryTutorial
local t = {2, 14, 5, 9}
table.sort(t)
print(unpack(t)) -- 2, 5, 9, 14
local mounts = {
	{id = 352, name = "Big Love Rocket"},
	{id = 435, name = "Mountain Horse"},
	{id = 464, name = "Azure Cloud Serpent"},
	{id = 522, name = "Sky Golem"},
}

-- ordena alfabéticamente por el campo de nombre
table.sort(mounts, function(a, b)
	return a.name < b.name
end)

for _, tbl in pairs(mounts) do
	print(tbl.id, tbl.name)
end
-- 464, Azure Cloud Serpent
-- 352, Big Love Rocket
-- 435, Mountain Horse
-- 522, Sky Golem
```
## Entorno Global
```lua
-- _G contiene el entorno global
-- puedes acceder explícitamente a las variables globales desde allí
apple = "madura"
local apple = "fresca"

print(apple)    -- fresca
print(_G.apple) -- madura

-- imprime todas las funciones globales en el entorno
for k, v in pairs(_G) do
	if type(v) == "function" then
		print(k, v)
	end
end
```
## Iteración con ipairs y pairs
```lua
-- ipairs() solo itera sobre pares índice-valor secuencialmente
local t = {
	[1] = "a",
	[2] = "b",
	[3] = "c",
	[27] = 123,
	fruit = "fresa",
	[33] = "hola",
}

for i, v in ipairs(t) do
	print(i, v)
end
-- 1, a
-- 2, b
-- 3, c

for k, v in pairs(t) do
	print(k, v)
end
-- 1, a
-- 2, b
-- 3, c
-- fruit, fresa
-- 33, hola
-- 27, 123
```
