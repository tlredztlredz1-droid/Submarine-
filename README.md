function CheckQuestNew()
    local _Value = game.Players.LocalPlayer.Data.Level.Value

    if _Value >= 2600 and _Value <= 2624 then
        MonNew = 'Reef Bandit'
        LevelQuestNew = 1
        NameQuestNew = 'SubmergedQuest1'
        NameMonNew = 'Reef Bandit'
        CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
        CFrameMonNew = CFrame.new(10736.6191, -2087.8439, 9338.4882)
    elseif _Value >= 2625 and _Value <= 2649 then
        MonNew = 'Coral Pirate'
        LevelQuestNew = 2
        NameQuestNew = 'SubmergedQuest1'
        NameMonNew = 'Coral Pirate'
        CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
        CFrameMonNew = CFrame.new(10965.1025, -2158.8842, 9177.2597)
    elseif _Value >= 2650 and _Value <= 2674 then
        MonNew = 'Sea Chanter'
        LevelQuestNew = 1
        NameQuestNew = 'SubmergedQuest2'
        NameMonNew = 'Sea Chanter'
        CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
        CFrameMonNew = CFrame.new(10621.0342, -2087.844, 10102.0332)
    elseif _Value >= 2675 and _Value <= 2750 then
        MonNew = 'Ocean Prophet'
        LevelQuestNew = 2
        NameQuestNew = 'SubmergedQuest2'
        NameMonNew = 'Ocean Prophet'
        CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
        CFrameMonNew = CFrame.new(11056.1445, -2001.6717, 10117.4493)
    else
        -- For levels above 2750 or below 2600
        if _Value > 2750 then
            MonNew = 'Ocean Prophet'
            LevelQuestNew = 2
            NameQuestNew = 'SubmergedQuest2'
            NameMonNew = 'Ocean Prophet'
            CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
            CFrameMonNew = CFrame.new(11056.1445, -2001.6717, 10117.4493)
        else
            MonNew = 'Reef Bandit'
            LevelQuestNew = 1
            NameQuestNew = 'SubmergedQuest1'
            NameMonNew = 'Reef Bandit'
            CFrameQuestNew = CFrame.new(10882.264, -2086.322, 10034.226)
            CFrameMonNew = CFrame.new(10736.6191, -2087.8439, 9338.4882)
        end
    end
end

spawn(function()
    while task.wait() do
        if _G.AutoFarmLevelNew then
            pcall(function()
                local _Quest = game:GetService('Players').LocalPlayer.PlayerGui.Main.Quest

                CheckQuestNew()

                if _Quest.Visible ~= false then
                    local MonFarm = nil
                    local FoundMon = false

                    -- Find the correct monster
                    for _, v in pairs(game:GetService('Workspace').Enemies:GetChildren()) do
                        if v.Name == MonNew and v:FindFirstChild('HumanoidRootPart') and v:FindFirstChild('Humanoid') and v.Humanoid.Health > 0 then
                            MonFarm = v
                            FoundMon = true
                            break
                        end
                    end

                    if FoundMon and MonFarm then
                        if string.find(_Quest.Container.QuestTitle.Title.Text, NameMonNew) then
                            repeat
                                task.wait()
                                EquipWeapon(_G.SelectWeapon)
                                AutoHaki()
                                topos(MonFarm.HumanoidRootPart.CFrame * CFrame.new(0, 30, 0))

                                MonFarm.HumanoidRootPart.CanCollide = false
                                MonFarm.Humanoid.WalkSpeed = 0
                                MonFarm.Head.CanCollide = false
                                MonFarm.HumanoidRootPart.Size = Vector3.new(70, 70, 70)
                                StartBring = true
                                MonFarmNew = MonFarm.Name

                                game:GetService('VirtualUser'):CaptureController()
                                game:GetService('VirtualUser'):Button1Down(Vector2.new(1280, 672))
                            until not _G.AutoFarmLevelNew or MonFarm.Humanoid.Health <= 0 or not MonFarm.Parent or _Quest.Visible == false

                            StartBring = false
                        else
                            StartBring = false
                            game:GetService('ReplicatedStorage').Remotes.CommF_:InvokeServer('AbandonQuest')
                        end
                    else
                        -- No monster found, go to spawn area
                        TP1(CFrameMonNew)
                        StartBring = false
                    end
                else
                    StartBring = false

                    if (game.Players.LocalPlayer.Character.HumanoidRootPart.Position - CFrameQuestNew.Position).Magnitude <= 20 then
                        game:GetService('ReplicatedStorage').Remotes.CommF_:InvokeServer('StartQuest', NameQuestNew, LevelQuestNew)
                    else
                        TP1(CFrameQuestNew)
                    end
                end
            end)
        end
    end
end)
