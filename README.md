# Bad Word Filter for Roblox
This repository includes known and/or unknown bypassed and unfiltered Roblox words and letters that developers can use for custom in-experience chat systems to detect inappropriate chat behavior from players that bypass Roblox's built-in text chat filters.

It's encouraged to always report inappropriate chat behavior to Roblox directly using the built-in [report abuse feature](https://en.help.roblox.com/hc/en-us/articles/203312410).

> [!NOTE]
> This filter list may subject to change at any given moment which may remove, add, or update existing or non-existing words in the list. This filter is also community-ran, meaning it is not 100% full proof nor is it 100% accurate.

## How can i use this in my experience?
You can start using the [blocklist file](https://raw.githubusercontent.com/MafuSaku/robloxbadwordfilter/main/blocklist.txt) inside of your Roblox experiences by storing the blocklist in a variable and updating that variable with the latest list every so often if needed. You can achieve this by using [`HttpService:GetAsync()`](https://create.roblox.com/docs/reference/engine/classes/HttpService#GetAsync) to retrieve the list in a table and splitting the words using [`string.split()`](https://create.roblox.com/docs/reference/engine/libraries/string#split).

An example in a server script would be
```lua
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")

local BLOCKLIST_URL = "https://raw.githubusercontent.com/MafuSaku/robloxbadwordfilter/main/blocklist.txt"
local blocklist

local function getBlocklist(url : string)
  if url then
		
    local success, response = pcall(HttpService.GetAsync, HttpService, url, false)
		
    if success and response then
      blocklist = string.split(response, ", ")
    else
      warn(`There was an error retrieving the blocklist! \nError: {response}`)
    end

  end
end

local function onPlayerAdded(player : Player)

  player.Chatted:Connect(function(message : string)
    if table.find(blocklist, message) then
      player:Kick("Please refrain from saying inappropriate words!")
    end
  end)
	
end

getBlocklist(BLOCKLIST_URL)

for _, player in Players:GetPlayers() do
  onPlayerAdded(player)
end

Players.PlayerAdded:Connect(onPlayerAdded)
```

## License
This repository is under the MIT License. Which can be reviewed [here](LICENSE).
