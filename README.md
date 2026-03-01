-- Script Path: game:GetService("ReplicatedStorage").Libraries.Classes.Ball
-- Took 0.02s to decompile.
-- Executor: Delta (1.1.708.880)

local v_u_1 = game:GetService("RunService")
local v2 = game:GetService("ReplicatedStorage")
local v_u_3 = game:GetService("CollectionService")
local v_u_4 = require(v2:WaitForChild("Libraries"):WaitForChild("Classes"):WaitForChild("Maid"))
local v_u_5 = game:GetService("Players")
game:GetService("ServerScriptService")
local v_u_6 = require(v2.Libraries.Modules.Collisions)
local v_u_7 = game:GetService("ServerStorage")
local v_u_8 = require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("Map"))
local v_u_9 = require(v2:WaitForChild("Libraries"):WaitForChild("Classes"):WaitForChild("VisualEffect"))
local v_u_10 = require(v2:WaitForChild("Libraries"):WaitForChild("Classes"):WaitForChild("SoundEffect"))
game:GetService("PhysicsService")
require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("NumberPlus"))
game:GetService("HapticService")
require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("TablePlus"))
local v_u_11 = require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("Profiles"))
local v_u_12 = require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("Replica"))
local v_u_13 = require(v2:WaitForChild("Libraries"):WaitForChild("Modules"):WaitForChild("Gamepasses"))
local v_u_14 = workspace.Map.Field.Main
local v_u_15 = v_u_1:IsServer()
local v_u_16 = v2:WaitForChild("Assets")
local v_u_17 = {
    ["balls"] = {},
    ["dribblePower"] = 0.2,
    ["lastSuper"] = {}
}
v_u_17.__index = v_u_17
function v_u_17.GetBall(p18, p19)
    if p19 then
        return p18.balls[p19]
    else
        return p18.balls
    end
end
function v_u_17.new(...)
    -- upvalues: (copy) v_u_17, (copy) v_u_4, (copy) v_u_11, (copy) v_u_15, (copy) v_u_7, (copy) v_u_6, (copy) v_u_3, (copy) v_u_1, (copy) v_u_8, (copy) v_u_10, (copy) v_u_9, (copy) v_u_5, (copy) v_u_12, (copy) v_u_14
    local v20 = v_u_17
    local v_u_21 = setmetatable({}, v20)
    local v22 = { ... }
    v_u_21.maid = v_u_4.new()
    v_u_21.ball = nil
    v_u_21.grace = false
    v_u_21.velocity = 0
    v_u_21.gameReplica = v_u_11:GetReplica("Game")
    v_u_21.active = true
    if v_u_15 then
        v_u_21.lastKick = nil
        v_u_21.lastKickPosition = nil
        v_u_21.lastKickType = nil
        v_u_21.kickers = {}
        v_u_21.visualId = nil
        v_u_21.visual = {}
        v_u_21.visualLocked = false
        v_u_21.ball = v_u_7.Assets.Ball:Clone()
        local v23 = v22[1] or workspace.Map.Field.PrimaryPart.CFrame * CFrame.new(0, v_u_21.ball.Ball.Size.Y / 2 + workspace.Map.Field.PrimaryPart.Size.Y / 2, 0)
        v_u_21:SetVisual(v22[2] or "Ball")
        v_u_21.ball:SetPrimaryPartCFrame(v23)
        v_u_21.ball.Parent = workspace
        v_u_21.maid.scoredEvent = Instance.new("BindableEvent")
        v_u_21.Scored = v_u_21.maid.scoredEvent.Event
        v_u_6:AddToGroup(v_u_21.ball, "Ball")
        v_u_3:AddTag(v_u_21.ball.Ball, "TimeScaleWhitelist")
        v_u_21.ball.Ball:SetNetworkOwner(nil)
        v_u_21.maid.dampenConnection = v_u_1.Heartbeat:Connect(function(p24)
            -- upvalues: (copy) v_u_21, (ref) v_u_8
            if v_u_21.ball.Ball.Position.Y < v_u_8:GetMap()[1].Position.Y + 5 then
                local v25 = v_u_21.ball.Ball
                v25.AssemblyAngularVelocity = v25.AssemblyAngularVelocity * (1 - p24 * 1.8)
            end
        end)
        v_u_21.goalieAutoReady = true
        v_u_21.maid.ballTouchConnection = v_u_21.ball.Ball.Touched:Connect(function(p26)
            -- upvalues: (ref) v_u_10, (ref) v_u_9, (copy) v_u_21
            if p26.Name == "Border" then
                v_u_10.new("Boundry"):Play()
                v_u_9.new("Boundry", v_u_21.ball.Ball.Position):Play()
                if v_u_21.lastKickType == "Dribble" then
                    local v27 = v_u_21.ball.Ball
                    local v28 = v27.AssemblyLinearVelocity
                    local v29 = v_u_21.ball.Ball.AssemblyLinearVelocity.Y
                    v27.AssemblyLinearVelocity = v28 - Vector3.new(0, v29, 0)
                    local v30 = v_u_21.ball.Ball
                    v30.AssemblyLinearVelocity = v30.AssemblyLinearVelocity * 1.1
                else
                    local v31 = v_u_21.ball.Ball
                    v31.AssemblyLinearVelocity = v31.AssemblyLinearVelocity + Vector3.new(0, 3, 0)
                    local v32 = v_u_21.ball.Ball
                    v32.AssemblyLinearVelocity = v32.AssemblyLinearVelocity * 1.1
                end
            elseif p26.Name == "Crossbar" then
                local v33 = v_u_21.ball.Ball
                v33.AssemblyAngularVelocity = v33.AssemblyAngularVelocity * 3
                v_u_10.new("Crossbar"):Play()
                v_u_9.new("Boundry", v_u_21.ball.Ball.Position):Play()
            end
        end)
        local v_u_34 = workspace.Map.Team1Goal
        local v_u_35 = workspace.Map.Team2Goal
        v_u_21.maid.goalConnection = v_u_1.Heartbeat:Connect(function()
            -- upvalues: (copy) v_u_21, (copy) v_u_34, (copy) v_u_35
            local v36 = v_u_21.ball.PrimaryPart.Position.Z
            local v37 = v_u_34.PrimaryPart.Position.Z
            local v38 = v_u_34.PrimaryPart.Size.Z * 2
            local v39 = v_u_34.PrimaryPart.Position.Z
            if v36 < v37 + v38 * math.sign(v39) then
                v_u_21.maid.scoredEvent:Fire("team2Score", v_u_21:GetKickerAndAssist("Red"))
                v_u_21.maid.goalConnection = nil
            else
                local v40 = v_u_21.ball.PrimaryPart.Position.Z
                local v41 = v_u_35.PrimaryPart.Position.Z
                local v42 = v_u_35.PrimaryPart.Size.Z * 2
                local v43 = v_u_35.PrimaryPart.Position.Z
                if v41 + v42 * math.sign(v43) < v40 then
                    v_u_21.maid.scoredEvent:Fire("team1Score", v_u_21:GetKickerAndAssist("Blue"))
                    v_u_21.maid.goalConnection = nil
                end
            end
        end)
        v_u_21.maid.ballPossession = v_u_1.Heartbeat:Connect(function(p44)
            -- upvalues: (copy) v_u_21, (ref) v_u_5, (ref) v_u_12
            local v45 = v_u_21:GetKickers()
            local v46 = v45[#v45]
            if v46 then
                local v47 = v_u_5:GetPlayerByUserId(v46)
                local v48
                if v47 then
                    local v49 = v47.UserId
                    v48 = tostring(v49)
                else
                    v48 = v47
                end
                if v47 then
                    v47 = v47.Character
                end
                if v47 then
                    v47 = v47:FindFirstChild("HumanoidRootPart")
                end
                if v47 and (v47.Position - v_u_21.ball.PrimaryPart.Position).Magnitude < 20 then
                    v_u_12:SetValue(v48, "BallPossession", p44)
                end
            end
        end)
        v_u_21.balls[v_u_21.ball] = v_u_21
        v_u_21.gameReplica:ArrayInsert("balls", v_u_21.ball)
    else
        v_u_21.ball = v22[1]
        v_u_21.balls[v_u_21.ball] = v_u_21
        v_u_21.maid.shadow = script.Shadow:Clone()
        v_u_21.maid.shadow.Parent = workspace
        v_u_21.maid.shadowConnection = v_u_1.Heartbeat:Connect(function()
            -- upvalues: (copy) v_u_21, (ref) v_u_8
            local v50 = v_u_21.ball.Ball.Position
            local v51 = RaycastParams.new()
            v51.FilterType = Enum.RaycastFilterType.Include
            v51.FilterDescendantsInstances = v_u_8:GetMap()
            local v52 = workspace:Raycast(v50, Vector3.new(0, -500, 0), v51)
            if v52 then
                local v53 = (v_u_21.ball.Ball.Position.Y - v52.Instance.Position.Y - v_u_21.ball.Ball.Size.Y / 2 - v52.Instance.Size.Y / 2) / 25
                local v54 = math.clamp(v53, 0, 0.8)
                v_u_21.maid.shadow.Transparency = v54
                v_u_21.maid.shadow.CFrame = CFrame.new(v52.Position) * CFrame.new(0, v_u_21.maid.shadow.Size.X / 2, 0) * CFrame.Angles(0, 0, 1.5707963267948966)
            end
        end)
    end
    v_u_21.prevPos = v_u_21.ball.Ball.Position
    v_u_21.yMagnitude = 0
    v_u_21.isGrounded = true
    v_u_21.maid.isGroundConnection = v_u_1.Heartbeat:Connect(function()
        -- upvalues: (copy) v_u_21, (ref) v_u_14
        local v55 = v_u_21.ball
        local v56
        if v55.PrimaryPart then
            v56 = v_u_14.Position.Y + v_u_14.Size.Y / 2 + v55.PrimaryPart.Size.Y / 2
        else
            v56 = nil
        end
        if v56 then
            if v_u_21.ball.Ball.Position.Y <= v56 then
                v_u_21.isGrounded = true
            else
                v_u_21.isGrounded = false
            end
        else
            return
        end
    end)
    v_u_21.maid.directionConnection = v_u_1.Heartbeat:Connect(function()
        -- upvalues: (copy) v_u_21
        v_u_21.yMagnitude = v_u_21.ball.Ball.Position.Y - v_u_21.prevPos.Y
        v_u_21.prevPos = v_u_21.ball.Ball.Position
    end)
    v_u_21.maid.velocityConnection = v_u_1.Heartbeat:Connect(function()
        -- upvalues: (copy) v_u_21
        v_u_21.velocity = v_u_21.ball.Ball.AssemblyLinearVelocity.Magnitude
    end)
    return v_u_21
end
function v_u_17.ApplyForce(p_u_57, p58, p59, p60, p61, p62)
    -- upvalues: (copy) v_u_11, (copy) v_u_12, (copy) v_u_10, (copy) v_u_6, (copy) v_u_1
    local v63
    if p60 == "Dribble" then
        v63 = p62 and 140 or 110
    elseif p60 == "Kick" then
        v63 = p62 and 100 or 90
    else
        v63 = p60 == "Header" and 50 or (p60 == "Goalie" and 50 or (p60 == "Slide" and 50 or nil))
    end
    local v64 = v63 * 1.05
    if v64 < p59.Magnitude then
        warn("Exploit detection: magnitude limit exceeded: " .. p59.Magnitude .. "/" .. v64)
    else
        local v65 = p58.Character
        if v65 then
            v65 = v65:FindFirstChild("HumanoidRootPart")
        end
        p_u_57.lastKick = tick()
        p_u_57.lastKickPosition = p_u_57.ball.PrimaryPart.Position
        p_u_57.lastKickType = p60
        if p_u_57.grace then
            p59 = v65.CFrame.LookVector * 55
        end
        if p60 == "Goalie" then
            local v66 = p_u_57:GetKickers()
            local v67 = v66[#v66]
            if v67 then
                v_u_11:GetReplica("Game")
                local v68 = v_u_11:GetReplica("Game").Data.playing[tostring(v67)]
                local v69 = v68
                if v69 then
                    v69 = v68.team
                end
                local v70 = p58.UserId
                local v71 = tostring(v70)
                v_u_11:GetReplica("Game")
                local v72 = v_u_11:GetReplica("Game").Data.playing[tostring(v71)]
                if v72 then
                    v72 = v72.team
                end
                print(v69, v72)
                if v69 and (v72 and (v69 ~= v72 and (v69 ~= "Neutral" and v72 ~= "Neutral"))) then
                    v_u_12:SetValue(p58.UserId, "Saves", 1)
                    v_u_12:SetValue(v67, "ShotsOnGoal", 1)
                end
            end
        end
        p_u_57.ball.Ball:SetNetworkOwner(nil)
        local v73 = p_u_57.yMagnitude * 20
        local v74 = math.abs(v73)
        local v75 = math.clamp(v74, 0, 15)
        local v76 = p59 + Vector3.new(0, v75, 0)
        p_u_57.ball.Ball.AssemblyLinearVelocity = v76
        p_u_57:ApplyCurve(p58, p61, v65.CFrame)
        p_u_57:SetKicker(p58)
        p_u_57:SetVisual(v_u_11:GetPlayerReplica(p58).Data.equipped.ball)
        v_u_10.new("Ball Kick1", p_u_57.ball.Ball.Position):Play()
        v_u_6:AddToGroup(p_u_57.ball, "BallNoCollision")
        local v_u_77 = 0
        p_u_57.maid.collisionConnection = v_u_1.Heartbeat:Connect(function(p78)
            -- upvalues: (ref) v_u_77, (ref) v_u_6, (copy) p_u_57
            v_u_77 = v_u_77 + p78
            if v_u_77 >= 0.3 then
                v_u_6:AddToGroup(p_u_57.ball, "Ball")
                p_u_57.maid.collisionConnection = nil
            end
        end)
        local v79 = p_u_57:GetKickers()
        local v80 = v79[#v79]
        local v81 = v79[#v79 - 1]
        v_u_11:GetReplica("Game")
        local v82 = v_u_11:GetReplica("Game").Data.playing[tostring(v80)]
        if v82 then
            v82 = v82.team
        end
        v_u_11:GetReplica("Game")
        local v83 = v_u_11:GetReplica("Game").Data.playing[tostring(v81)]
        if v83 then
            v83 = v83.team
        end
        if v81 and (v80 and (v81 ~= v80 and (v83 == v82 and (v82 ~= "Neutral" and v83 ~= "Neutral")))) then
            v_u_12:SetValue(v81, "Passes", 1)
        end
    end
end
function v_u_17.ApplyCurve(p_u_84, p85, p_u_86, p_u_87)
    -- upvalues: (copy) v_u_13
    if p_u_86 then
        local v_u_88 = v_u_13:Owns(p85, 9864487)
        local v_u_89 = false
        local v90 = p_u_86 == "Right" and Vector3.new(0, -10, 0) or (p_u_86 == "Left" and Vector3.new(0, 10, 0) or false)
        if v_u_88 then
            v90 = v90 * 1.5
        end
        p_u_84.ball.Ball.AssemblyAngularVelocity = v90
        p_u_84.maid.borderTouchConnection = p_u_84.ball.Ball.Touched:Connect(function(p91)
            -- upvalues: (ref) v_u_89
            if p91.Name == "Border" then
                v_u_89 = true
            end
        end)
        task.spawn(function()
            -- upvalues: (copy) p_u_84, (copy) p_u_86, (copy) p_u_87, (copy) v_u_88
            local v92 = 0
            while v92 < 12 and p_u_84:IsActive() do
                v92 = v92 + 1
                local v93 = p_u_86 == "Right" and p_u_87.RightVector
                if not v93 then
                    if p_u_86 == "Left" then
                        v93 = -p_u_87.RightVector
                    else
                        v93 = false
                    end
                end
                local v94 = v93 * (v92 / 10 + 0.5) * 1.5
                if v_u_88 then
                    v94 = v94 * 1.8
                end
                local v95 = p_u_84.ball.Ball
                v95.AssemblyLinearVelocity = v95.AssemblyLinearVelocity + v94
                task.wait(0.15)
            end
            p_u_84.maid.borderTouchConnection = nil
        end)
    end
end
function v_u_17.SetKicker(p96, p97)
    local v98 = p96.kickers
    local v99 = p97.UserId
    local v100 = tostring(v99)
    table.insert(v98, v100)
end
function v_u_17.GetKickers(p101)
    return p101.kickers
end
function v_u_17.SetGrace(p_u_102, p_u_103, p_u_104)
    p_u_102.grace = p_u_103
    if p_u_104 then
        task.spawn(function()
            -- upvalues: (copy) p_u_104, (copy) p_u_102, (copy) p_u_103
            task.wait(p_u_104)
            p_u_102.grace = not p_u_103
        end)
    end
end
function v_u_17.SetVisual(p105, p106)
    -- upvalues: (copy) v_u_16
    local v107 = v_u_16.ball:FindFirstChild(p106)
    if p105.visualLocked then
        return
    elseif v107 then
        for _, v108 in ipairs(v107.Ball:GetChildren()) do
            local v109 = v108:Clone()
            v109.Parent = p105.ball.Ball
            local v110 = p105.visual
            table.insert(v110, v109)
        end
        p105.visualId = p106
    end
end
function v_u_17.LockVisual(p111)
    p111.visualLocked = true
end
function v_u_17.RemoveVisual(p112)
    for _, v113 in ipairs(p112.visual) do
        v113:Destroy()
    end
    p112.visual = {}
end
function v_u_17.GetKickerAndAssist(p114, p115)
    -- upvalues: (copy) v_u_11
    local v116 = p114:GetKickers()
    local v117 = nil
    local v118 = nil
    for v119 = #v116, 1, -1 do
        local v120 = v116[v119]
        v_u_11:GetReplica("Game")
        local v121 = v_u_11:GetReplica("Game").Data.playing[tostring(v120)]
        if v121 then
            v121 = v121.team
        end
        if v121 == p115 then
            if v117 then
                if v118 then
                    break
                end
                if v120 ~= v117 then
                    v118 = v120
                end
            else
                v117 = v120
            end
        end
    end
    return v117, v118
end
local function v_u_130(p122, p123, p124)
    -- upvalues: (copy) v_u_8
    if p123 then
        local v125 = math.clamp(p123, 0.2, 1)
        local v126 = p122.Position + p122.CFrame.LookVector * (v125 * (p124 and 90 or 70))
        local v127 = v126.X
        local v128 = v_u_8:GetMap()[1].Position.Y + v_u_8:GetMap()[1].Size.Y / 2
        local v129 = v126.Z
        return Vector3.new(v127, v128, v129)
    end
end
function v_u_17.Kick(p131, p132, p133, p134, p135)
    -- upvalues: (copy) v_u_5, (copy) v_u_17, (copy) v_u_130
    local v136 = v_u_5.LocalPlayer
    if v136 then
        v136 = v136.Character
    end
    local v137
    if v136 then
        v137 = v136:FindFirstChild("HumanoidRootPart")
    else
        v137 = v136
    end
    if v136 then
        v136:FindFirstChild("Humanoid")
    end
    local v138 = p132 and p132 < v_u_17.dribblePower and "Dribble" or p133
    if v138 == "Dribble" then
        local v139 = v137.CFrame.LookVector * (p135 and 60 or 45)
        p131.gameReplica:FireServer("Ball", p131.ball, v139, "Dribble", p134, p135)
        return
    elseif v138 == "Ground" then
        local v140 = v_u_130(v137, p132, p135)
        local v141 = v137.CFrame.LookVector * ((v137.Position - v140).Magnitude + (p135 and 50 or 40))
        p131.gameReplica:FireServer("Ball", p131.ball, v141, "Dribble", p134, p135)
        return
    elseif v138 == "Goalie" then
        local v142 = v137.CFrame.LookVector * 40 + Vector3.new(0, 20, 0)
        p131.gameReplica:FireServer("Ball", p131.ball, v142, "Goalie", p134)
        return
    elseif v138 == "Slide" then
        local v143 = v137.CFrame.LookVector * 40 + Vector3.new(0, 10, 0)
        p131.gameReplica:FireServer("Ball", p131.ball, v143, "Slide")
    elseif v138 == "Kick" then
        local v144 = -game.Workspace.Gravity
        local v145 = Vector3.new(0, v144, 0)
        local v146 = p131.ball.Ball.Position
        local v147 = v_u_130(v137, p132, p135)
        local v148 = p135 and 1 or Random.new():NextNumber(90, 105) / 100
        local v149 = 1.5 * p132
        local v150 = math.clamp(v149, 0.2, 1) * v148
        local v151 = (v147 - v146 - 0.5 * v145 * v150 * v150) / v150
        p131.gameReplica:FireServer("Ball", p131.ball, v151, "Kick", p134, p135)
    end
end
function v_u_17.Header(p152)
    -- upvalues: (copy) v_u_5
    local v153 = v_u_5.LocalPlayer
    if v153 then
        v153 = v153.Character
    end
    if v153 then
        v153 = v153:FindFirstChild("HumanoidRootPart")
    end
    local v154 = v153.CFrame.LookVector * 45 + Vector3.new(0, 7, 0)
    p152.gameReplica:FireServer("Ball", p152.ball, v154, "Header")
end
function v_u_17.IsActive(p155)
    return p155.active
end
function v_u_17.Destroy(p156)
    -- upvalues: (copy) v_u_15, (copy) v_u_17
    if v_u_15 then
        local v157 = table.find(p156.gameReplica.Data.balls, p156.ball)
        p156.gameReplica:ArrayRemove("balls", v157)
        p156.ball:Destroy()
        v_u_17.balls[p156.ball] = nil
    else
        v_u_17.balls[p156.ball] = nil
    end
    p156.active = false
    p156.maid:Destroy()
end
return v_u_17
