local HttpService = game:GetService("HttpService")
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")

-- Replace with your Discord Webhook URL
local webhookURL = "https://discord.com/api/webhooks/1310183374836596837/APVkL8us47QhIL2k2l3QZPwutzWsAaA0-Ifrhqc20Fx-thpA6SBay_2ZCDyiC5Hh_Eut"

-- Function to check if it is Full Moon
local function isFullMoon()
    if Lighting:FindFirstChild("MoonTextureId") then
        local moonTexture = Lighting.MoonTextureId
        -- Check if the Moon is Full Moon (Full Moon ID)
        if moonTexture == "rbxassetid://7040776556" then -- Full Moon ID
            return true
        end
    end
    return false
end

-- Function to calculate the time left for Full Moon (assuming Full Moon lasts 20 minutes)
local function timeLeftForFullMoon()
    -- Assume Full Moon lasts 20 minutes
    local fullMoonDuration = 20 * 60 -- 20 minutes in seconds
    local timeElapsed = tick() % fullMoonDuration  -- Get the elapsed time in the Full Moon cycle

    return fullMoonDuration - timeElapsed
end

-- Function to send a notification to Discord
local function sendDiscordNotification(playerCount, timeLeft)
    local minutesLeft = math.floor(timeLeft / 60)
    local message = {
        ["content"] = "🌕 Full Moon has appeared in Blox Fruits!\n" ..
                      "Current player count: " .. playerCount .. "\n" ..
                      "Time left for Full Moon: " .. minutesLeft .. " minutes."
    }
    local jsonData = HttpService:JSONEncode(message)

    -- Send notification to Discord
    HttpService:PostAsync(webhookURL, jsonData, Enum.HttpContentType.ApplicationJson)
end

-- Main function to check for Full Moon and send notifications
while true do
    if isFullMoon() then
        local playerCount = #Players:GetPlayers()  -- Count the number of players in the server
        local timeLeft = timeLeftForFullMoon()      -- Calculate time left for Full Moon

        -- Send notification to Discord
        sendDiscordNotification(playerCount, timeLeft)

        -- Send in-game notification
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Notification!",
            Text = "🌕 Full Moon has appeared in the game!\n" ..
                   "Current player count: " .. playerCount .. "\n" ..
                   "Time left for Full Moon: " .. math.floor(timeLeft / 60) .. " minutes.",
            Duration = 10
        })

        -- Wait until Full Moon ends, check again after 60 seconds (1 minute)
        wait(60)
    else
        -- Wait a minute before checking again if it's not Full Moon
        wait(60)
    end
end
