getgenv().Time = 0.1

getgenv().Head = {
    83491257029720, -- Hat accessory ID
}

getgenv().Face = {
    111238191940165, -- Replace with Face accessory ID
}

getgenv().Torso = {
    17153974185, -- Torso accessory ID
}

getgenv().RightArm = {
    17268244809 -- Right Arm accessory ID
}

getgenv().LeftArm = {
    17268241691 -- Left Arm accessory ID
}

local function weldParts(part0, part1, c0, c1)
    local weld = Instance.new("Weld")
    weld.Part0 = part0
    weld.Part1 = part1
    weld.C0 = c0
    weld.C1 = c1
    weld.Parent = part0
    return weld
end

local function addAccessoryToCharacter(accessoryId, parentPart)
    local accessory = game:GetObjects("rbxassetid://" .. tostring(accessoryId))[1]
    local character = game.Players.LocalPlayer.Character

    accessory.Parent = game.Workspace

    local handle = accessory:FindFirstChild("Handle")
    if handle then
        handle.CanCollide = false
        local attachment = handle:FindFirstChildOfClass("Attachment")
        if attachment then
            local parentAttachment = parentPart:FindFirstChild(attachment.Name)
            if parentAttachment then
                weldParts(parentPart, handle, parentAttachment.CFrame, attachment.CFrame)
            end
        end
    end

    accessory.Parent = character
end

local function loadAccessories(character)
    for _, accessoryId in ipairs(getgenv().Head) do
        addAccessoryToCharacter(accessoryId, character:FindFirstChild("Head"))
    end

    for _, accessoryId in ipairs(getgenv().Face) do
        addAccessoryToCharacter(accessoryId, character:FindFirstChild("Head"))
    end

    local torsoPart = character:FindFirstChild("UpperTorso") or character:FindFirstChild("Torso")
    if torsoPart then
        for _, accessoryId in ipairs(getgenv().Torso) do
            addAccessoryToCharacter(accessoryId, torsoPart)
        end
    end

    local rightArm = character:FindFirstChild("RightUpperArm") or character:FindFirstChild("Right Arm")
    if rightArm then
        for _, accessoryId in ipairs(getgenv().RightArm) do
            addAccessoryToCharacter(accessoryId, rightArm)
        end
    end

    local leftArm = character:FindFirstChild("LeftUpperArm") or character:FindFirstChild("Left Arm")
    if leftArm then
        for _, accessoryId in ipairs(getgenv().LeftArm) do
            addAccessoryToCharacter(accessoryId, leftArm)
        end
    end
end

local function onCharacterAdded(character)
    -- Ensure the character is fully loaded before proceeding
    character:WaitForChild("Head")
    character:WaitForChild("UpperTorso") or character:WaitForChild("Torso")
    character:WaitForChild("RightUpperArm") or character:WaitForChild("Right Arm")
    character:WaitForChild("LeftUpperArm") or character:WaitForChild("Left Arm")

    wait(getgenv().Time)
    loadAccessories(character)
end

-- Connect to the character added event to trigger loading accessories
game.Players.LocalPlayer.CharacterAdded:Connect(onCharacterAdded)
