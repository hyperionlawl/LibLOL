# Finity Docs

## Creating a basic UI with Finity
In this basic tutorial, we'll be learning how to create a basic menu with examples for how to use each cheat module.

## Prerequisites
- An exploit that can use game:HttpGet()
- Basic Lua syntax knowledge
- Ability to read
- A few spare brain cells
- Patience

## Loading Finity into your script
First things first, load the library using loadstringand assign the returned value to a variable of your choice. 
Or, if you don't understand what I just said, feel free to skid this onto your text editor:
```
local Finity = loadstring(game:HttpGet("http://finity.vip/scripts/finity_lib.lua"))()
```
## Creating a FinityWindow for your UI elements
Then, you're going to need to create a new window to house all of your amazing UI elements! Make sure you have the library above included in your code. Preferably before the constructor is called.
 
Alongside the creation of the window, we're also going to need to set a ToggleKey. A ToggleKey allows the user to press a key to hide or show the menu. Pretty simple.
You can do that like so:
```
local FinityWindow = Finity.new(false)
FinityWindow.ChangeToggleKey(Enum.KeyCode.Semicolon)
```

If you would like to change the key that shows/hides the menu, change "Semicolon" to a valid KeyCode.
### Additionally, f you would like to use dark mode for your UI, change the false in the constructor to true
​
## Creating some categories
After that, you need to create some categories. Let's create two categories: "Visuals"and "Aimbot". I've named the variables accordingly as it helps with organization later on:
```
local VisualsCategory = FinityWindow:Category("Visuals")
local AimbotCategory = FinityWindow:Category("Aimbot")
```
## Creating sectors for categories
Now that we have our categories, it's time to create sectors for each of them. I recommend separating them into sub-categories. 
For example: if you have a "Visuals" tab, maybe have sectors like "Player ESP", "Zombie ESP", "Item ESP".

Let's create some sectors for our Visuals category. Again, I've also named the variabeld accordingly as it helps with organization later on. It's important to do this because you dont want to end up with variables like ESP1 or ESP2 as can get really confusing after a while.
```
-- Visuals Sectors
local VisualsESPSettings = VisualsCategory:Sector("ESP Settings")
local VisualsPlayerESP = VisualsCategory:Sector("Player ESP")
local VisualsItemESP = VisualsCategory:Sector("Item ESP")
We can repeat the same process for our Aimbot category.
-- Aimbot Sectors
local AimbotColors = AimbotCategory:Sector("Aimbot Colors")
local AimbotHotkeys = AimbotCategory:Sector("Aimbot Hotkeys")
local AimbotConfigurations = AimbotCategory:Sector("Aimbot Configurations")
```
And voilà! Now we have two categories, each with their respective sectors.

## Creating and implementing cheats
"Where are the cheats?!?!?!?" is something you might be wondering. Well, we haven't created any yet! I'll talk about how to create each one and the "special" things you can do to them.

## Checkboxes
Now, checkboxes are next to nothing when it comes to complexity. You have a button that toggles something to be either true or false.  This was the first thing that I added into Finity. Why? I don't really know.
To create a checkbox (or literally any cheat module), all you have to do is run the :Cheat() method on any sector (with the correct parameters)!
Here, I'm going to create an Enable toggle for each of the sectors that require one:
```
VisualsESPSettings:Cheat(
	"Checkbox", -- Type
	"ESP Enabled", -- Name
	function(State) -- Callback function
		print("Checkbox state changed:", State)
	end
)
VisualsPlayerESP:Cheat(
	"Checkbox", -- Type
	"Player ESP Enabled", -- Name
	function(State) -- Callback function
		print("Checkbox state changed:", State)
	end
)
VisualsItemESP:Cheat(
	"Checkbox", -- Type
	"Item ESP Enabled", -- Name
	function(State) -- Callback function
		print("Checkbox state changed:", State)
	end
)
​
AimbotConfigurations:Cheat(
	"Checkbox", -- Type
	"Aimbot Enabled", -- Name
	function(State) -- Callback function
		print("Checkbox state changed:", State)
	end
)
If you want to reduce space, you can condense cheats into a lot less space like so:
-- Uncondensed
VisualsESPSettings:Cheat(
	"Checkbox", -- Type
	"ESP Enabled", -- Name
	function(State) -- Callback function
		print("Checkbox state changed:", State)
	end
)
​
-- Condensed
VisualsESPSettings:Cheat("Checkbox", "ESP Enabled", function(State)
	print("Checkbox state changed:", State)
end)
```
This here creates checkboxes under each of that categories that require one. Here's a list of parameters for the :Cheat() method
Sliders

## Sliders
Unlike checkboxes, **sliders** offer a more specific way of choosing a value, allowing the user to choose between a custom range of numbers. 
Let's add sliders with reasonable ranges to reasonable places:
```
VisualsESPSettings:Cheat("Slider", "Render Distance", function(Value)
	print("Silder value changed:", Value)
end, {min = 0, max = 1500, suffix = " studs"})
​
AimbotConfigurations:Cheat("Slider", "Aimbot FOV", function(Value)
	print("Silder value changed:", Value)
end, {min = 0, max = 120, suffix = "°"})
```

I've condensed the code as well as added minimum values, maximum values and suffixes for the sliders. In this code sample, you can see how setting custom cheat data will be used. All you have to do is give a table of data values to assign to the cheat in an array as the 4th argument. It sound like a lot, I know, but it's really not. The best way to learn is to read, edit and attempt to make sense of things you don't understand.

## Dropdowns

Dropdowns are a handy way to get the user to choose from a table of options. When using dropdowns, make sure to include an options array in the data table in the 4th parameter. 
Let's add a couple of these to our window by adding the following code:
```
VisualsESPSettings:Cheat("Dropdown", "ESP Color", function(Option)
	print("Dropdown option changed:", Option)
end, {
	options = {
		"Red",
		"White",
		"Green",
		"Pink",
		"Blue"
	}
})
​
AimbotConfigurations:Cheat("Dropdown", "Aimbot Mode", function(Option)
	print("Dropdown option changed:", Option)
end, {
	options = {
		"FOV",
		"Distance",
		"Visibility"
	}
})
The default option is always the first one in the table of options. You can change this by specifying a default value in the data array like so:
{
	options = {
		"FOV",
		"Distance",
		"Visibility"
	}
	default = "Distance"
}
```

## Textboxes

Want a user to be able to enter some text into your cheat for very custom values? Textboxes are the way to go. You can also set the placeholder of the textbox so your user knows what they're entering.

A good use for a textbox would be searching for a player to teleport to or looping through an array of things for a given string.
Lastly, we'll implement some textboxes into our sectors:
```
-- Visuals Textboxes
VisualsItemESP:Cheat("Textbox", "Item To Whitelist", function(Value)
	print("Textbox value changed:", Value)
end, {
	placeholder = "Item Name"
})
VisualsPlayerESP:Cheat("Textbox", "Player To Whitelist", function(Value)
	print("Textbox value changed:", Value)
end, {
	placeholder = "Player Name"
})
​
-- Aimbot Textboxes
AimbotColors:Cheat("Textbox", "BrickColor Input", function(Value)
	print("Textbox value changed:", Value)
end, {
	placeholder = "BrickColor"
})
AimbotHotkeys:Cheat("Textbox", "Quick Toggle Hotkey", function(Value)
	print("Textbox value changed:", Value)
end, {
	placeholder = "KeyCode"
})
AimbotHotkeys:Cheat("Textbox", "Panic Hotkey", function(Value)
	print("Textbox value changed:", Value)
end, {
	placeholder = "KeyCode"
})
```
Pretty neat, right? Textboxes are surely the joy of the world 

 
## Buttons

Another nice feature that Finity offers is buttons. Buttons allow for a user to fire the callback without any arguments.
That's pretty much it. There's no extra data that you need to add!
Now, let's create a some buttons where reasonable:
```
VisualsPlayerESP:Cheat("Button", "Reset Whitelist", function()
	print("Button pressed")
end)
​
AimbotColors:Cheat("Button", "Reset Color", function()
	print("Button pressed")
end)
​
AimbotHotkeys:Cheat("Button", "Reset Key", function()
	print("Button pressed")
end)
```
## Labels

Last but not least, labels are a good way of giving some extra information to the user about the cheat, or to use as section spacers.
Let's give ourselves some credit by creating a new category and some new sectors:
```
-- Create category
local CreditsCategory = FinityWindow:Category("Credits")
​
-- Create sectors
local CreditsCreator = CreditsCategory:Sector("Finity Library Creator")
local CreditsSpecialThanks = CreditsCategory:Sector("Special Thanks")
local CreditsTesters = CreditsCategory:Sector("Testers")
​
-- Create labels
CreditsCreator:Cheat("Label", "detourious @ v3rmillion.net")
CreditsCreator:Cheat("Label", "deto#7612 @ discord.gg")
​
CreditsSpecialThanks:Cheat("Label", "wallythebird - held me hostage")
CreditsSpecialThanks:Cheat("Label", "Jan - some inspiration from his lib showcase")
CreditsSpecialThanks:Cheat("Label", "& all of you for supporting me <3")
​
CreditsTesters:Cheat("Label", "detourious - made the darn thing")
```
Now that we have covered everything, let's take a look at the final product!
Congratulations! You have made a very nice & sleek UI. Everything works fine & you can now share this with your fellow V3rmies!

# Summary
So, now that you're finished with this, take a moment to think about what you learned. There was quite a lot to cover. Also, I don't have anything wrong with copy-pasting the code that was written here. If anyone calls you a skid then send them this, because you have full permission to Finity & any code written on this web-page anyway you would like. Use it to make your own script hub, release cool cheats to others, sell premium scripts, I really don't care.

# So, what should I have learned?
You should have learned mostly everything there is to offer with Finity and how to use it to create your own epic & hot user interfaces. Here's a run-down of everything covered in this article:

= Creating UI elements using Finity
= Categories,
= Sectors,
= Each type of cheat module,
= Usefulness of each type of cheat module
= Creating fully-functioning user interfaces using Finity
# This is the only version of Finity avaliable on this repo. 

# Q&A:
Can I use your library any way I like (even monetization)?
Yes! You are allowed to use Finity however you would like. The one thing I ask is for people to tell their interested friends or communities about Finity.
Another thing I ask of you is to not claim this as your own and/or sell this library. It's free-to-use for everybody & should remain that way.

# CREDITS

```
LIBRARY : EVERY RIGHT GOES TO ORIGINAL CREATOR
UPDATED AND BETTER DOCUMENTATION : hyperionlawl @ discord.com
```

## FOOTNOTE: THIS ENTIRE REPO IS FREE TO FORK & MODIFY <33
## PULL REQS FOR THIS REPO WONT BE CHECKED :) 
## DM hyperionlawl ON DISCORD IF YOU WISH TO BE A REPO CONTRIBUTOR
