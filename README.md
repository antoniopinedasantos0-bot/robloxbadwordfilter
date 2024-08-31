# Bad Word Filter for Roblox
This repository includes known and/or unknown bypassed and unfiltered Roblox words that developers can use for custom in-experience chat systems to detect inappropriate chat behavior from players that bypasses the Roblox's built-in text chat filters.

> [!NOTE]
> This filter list may subject to change at any given moment which may remove, add, or update existing or non-existing words in the list. This filter is also community-ran, meaning it is not 100% full proof nor is it 100% accurate.

## How can i use this in my experience?
You can start using the [blocklist file](https://raw.githubusercontent.com/MafuSaku/robloxbadwordfilter/main/blocklist.txt) inside of your Roblox experiences by storing the blocklist in a variable and updating that variable with the latest list every 60 seconds. You can achieve this by using [`HttpService:GetAsync()`](https://create.roblox.com/docs/reference/engine/classes/HttpService#GetAsync) to retrieve the list in a table and splitting the words using [`string.split()`](https://create.roblox.com/docs/reference/engine/libraries/string#split).

An example in a server script would be
```lua
local HttpService = game:GetService("HttpService")
local Players = game:GetService("Players")

local BLOCKLIST_URL : string = "https://raw.githubusercontent.com/MafuSaku/robloxbadwordfilter/main/blocklist.txt"
local blocklist : {}

local function getBlockList(blocklistUrl : string)  
  local getSuccess, getResult = pcall(HttpService.GetAsync, HttpService, blocklistUrl, false)
  
  if getSuccess == false then
    warn(`There was an error retrieving the blocklist: {getResult}`)
    return
  else
    blocklist = string.split(", ", getResult)
  end

end

-- Kicks the player if found saying a word on the list
local function onPlayerAdded(player: Player)
  player.Chatted:Connect(function(message: string)
    if table.find(blocklist, message) then
      player:Kick("You said a no no word!")
    end
  end)
end

getBlockList()

for _, player in Players:GetPlayers() do
  onPlayerAdded(player)
end

Players.PlayerAdded:Connect(onPlayerAdded)
```

## License
This repository is under the MIT License. Which can be reviewed [here](#LICENSE).
