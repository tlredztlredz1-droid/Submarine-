--// QUEST CHECK
local function CheckQuestNew()

    local Data = player:FindFirstChild("Data")
    if not Data then
        return
    end

    local Level = Data:FindFirstChild("Level")
    if not Level then
        return
    end

    local I = Level.Value

    --// 2600-2624
    if I >= 2600 and I <= 2624 then

        MonNew = "Reef Bandit"

        LevelQuestNew = 1
        NameQuestNew = "SubmergedQuest1"

        CFrameQuestNew = CFrame.new(
            10778.875,
            -2087.72437,
            9265.18359
        )

        CFrameMonNew = CFrame.new(
            11019.1318,
            -2146.06812,
            9342.3916
        )

    --// 2625-2649
    elseif I >= 2625 and I <= 2649 then

        MonNew = "Coral Pirate"

        LevelQuestNew = 2
        NameQuestNew = "SubmergedQuest1"

        CFrameQuestNew = CFrame.new(
            10778.875,
            -2087.72437,
            9265.18359
        )

        CFrameMonNew = CFrame.new(
            10808.6006,
            -2030.36145,
            9364.2334
        )

    --// 2650-2674
    elseif I >= 2650 and I <= 2674 then

        MonNew = "Sea Chanter"

        LevelQuestNew = 1
        NameQuestNew = "SubmergedQuest2"

        CFrameQuestNew = CFrame.new(
            10880.6855,
            -2086.20044,
            10032.624
        )

        CFrameMonNew = CFrame.new(
            10671.2715,
            -2057.59155,
            10047.2588
        )

    --// 2675-2699
    elseif I >= 2675 and I <= 2699 then

        MonNew = "Ocean Prophet"

        LevelQuestNew = 2
        NameQuestNew = "SubmergedQuest2"

        CFrameQuestNew = CFrame.new(
            10880.6855,
            -2086.20044,
            10032.624
        )

        CFrameMonNew = CFrame.new(
            11008.5195,
            -2007.72839,
            10223.0791
        )

    --// 2700-2724
    elseif I >= 2700 and I <= 2724 then

        MonNew = "High Disciple"

        LevelQuestNew = 1
        NameQuestNew = "SubmergedQuest3"

        CFrameQuestNew = CFrame.new(
            9640.08789,
            -1992.44507,
            9613.65234
        )

        CFrameMonNew = CFrame.new(
            9750.41602,
            -1966.93884,
            9753.36035
        )

    --// 2725+
    elseif I >= 2725 then

        MonNew = "Grand Devotee"

        LevelQuestNew = 2
        NameQuestNew = "SubmergedQuest3"

        CFrameQuestNew = CFrame.new(
            9640.08789,
            -1992.44507,
            9613.65234
        )

        CFrameMonNew = CFrame.new(
            9611.70508,
            -1993.47119,
            9882.68848
        )
    end

    local hrp = Character():FindFirstChild("HumanoidRootPart")

    if hrp and CFrameQuestNew then

        local Distance =
            (hrp.Position - CFrameQuestNew.Position).Magnitude

        if Distance > 10000 then
            TeleportSubmerged()
        end
    end
end

--// GET ENEMY MAIS INTELIGENTE
local function GetEnemy()

    local Enemies = Workspace:FindFirstChild("Enemies")

    if not Enemies then
        return nil
    end

    local Closest = nil
    local Distance = math.huge

    local hrp = Character():FindFirstChild("HumanoidRootPart")

    if not hrp then
        return nil
    end

    for _, v in pairs(Enemies:GetChildren()) do

        if v.Name == MonNew
        and v:FindFirstChild("Humanoid")
        and v:FindFirstChild("HumanoidRootPart")
        and v.Humanoid.Health > 0 then

            local mag =
                (hrp.Position - v.HumanoidRootPart.Position).Magnitude

            if mag < Distance then
                Distance = mag
                Closest = v
            end
        end
    end

    return Closest
end

--// EQUIP WEAPON
local function EquipWeapon()

    pcall(function()

        local char = Character()

        if char:FindFirstChild(_G.SelectWeapon) then
            return
        end

        local Tool =
            player.Backpack:FindFirstChild(_G.SelectWeapon)

        if Tool then
            char.Humanoid:EquipTool(Tool)
        end
    end)
end

--// AUTO HAKI LOOP
task.spawn(function()
    while task.wait(1) do
        pcall(function()

            if not _G.AutoFarmLevelNew then
                return
            end

            local char = Character()

            if not char:FindFirstChild("HasBuso") then
                ReplicatedStorage.Remotes.CommF_:InvokeServer("Buso")
            end
        end)
    end
end)

--// MAIN LOOP
task.spawn(function()

    while task.wait(0.1) do

        pcall(function()

            if not _G.AutoFarmLevelNew then
                return
            end

            CheckQuestNew()

            if not MonNew then
                return
            end

            local char = Character()
            local hrp = char:FindFirstChild("HumanoidRootPart")

            if not hrp then
                return
            end

            local MainGui =
                player.PlayerGui:FindFirstChild("Main")

            local QuestGui =
                MainGui and MainGui:FindFirstChild("Quest")

            local CommF_ =
                ReplicatedStorage:WaitForChild("Remotes")
                :WaitForChild("CommF_")

            -- QUEST ATIVA
            if QuestGui and QuestGui.Visible then

                local Enemy = GetEnemy()

                if Enemy then

                    local EnemyHRP =
                        Enemy:FindFirstChild("HumanoidRootPart")

                    local EnemyHum =
                        Enemy:FindFirstChild("Humanoid")

                    if EnemyHRP and EnemyHum then

                        repeat
                            task.wait()

                            if not _G.AutoFarmLevelNew then
                                break
                            end

                            EquipWeapon()

                            EnemyHRP.CanCollide = false
                            EnemyHRP.Size = Vector3.new(80,80,80)

                            EnemyHum.WalkSpeed = 0
                            EnemyHum.JumpPower = 0

                            -- FICA PARADO NO AR
                            hrp.CFrame =
                                EnemyHRP.CFrame *
                                CFrame.new(0,_G.FarmHeight,0)

                            hrp.Velocity = Vector3.zero

                            VirtualUser:CaptureController()
                            VirtualUser:Button1Down(
                                Vector2.new(500,500)
                            )

                        until EnemyHum.Health <= 0
                        or not Enemy.Parent
                        or not QuestGui.Visible

                    end

                else
                    TweenTP(CFrameMonNew)
                end

            else

                local Distance =
                    (hrp.Position - CFrameQuestNew.Position).Magnitude

                if Distance <= 20 then

                    CommF_:InvokeServer(
                        "StartQuest",
                        NameQuestNew,
                        LevelQuestNew
                    )

                    task.wait(1)

                else
                    TweenTP(CFrameQuestNew)
                end
            end
        end)
    end
end)
