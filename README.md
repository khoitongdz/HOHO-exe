-- Kenon Hub V2 (No Key Version)
-- Edited by khoitongdz

local function LoadKenonHubV2()
    local success, result = pcall(function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/acsu123/HohoV2/main/NewMain"))()
    end)
    if success then
        print("Kenon Hub V2 Loaded Successfully!")
    else
        warn("Failed to load Kenon Hub V2: ", result)
    end
end

-- Execute the function
LoadKenonHubV2()
