
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")

local player = Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")
local hrp = char:WaitForChild("HumanoidRootPart")

local mask = player:WaitForChild("PlayerGui"):WaitForChild("AbilityBars"):WaitForChild("ChronalMask")
local dashEvent = mask:WaitForChild("ChronalDashEvent")

-- Configs
local HoldTime = 2.5
local TapDuration = 0.3
local TeleportDistance = 30
local WarpColor = Color3.fromRGB(50, 50, 80)
local WarpSaturation = -1
local WarpContrast = 1.5
local WarpTint = Color3.fromRGB(0, 0, 255)
local WarpSoundId = "rbxassetid://"
local MaxBlur = 20

-- States
local holding = false
local activated = false

-- Helpers
local function send(action)
	dashEvent:Fire(action)
end

local function createEffect()
	local blur = Instance.new("BlurEffect")
	local cc = Instance.new("ColorCorrectionEffect")
	local sound = Instance.new("Sound")

	blur.Size = 0
	cc.TintColor = WarpTint
	cc.Saturation = WarpSaturation
	cc.Contrast = WarpContrast
	sound.SoundId = WarpSoundId
	sound.Volume = 1.2
	sound.PlaybackSpeed = 1.1
	sound.Parent = hrp
	blur.Parent = Lighting
	cc.Parent = Lighting

	-- Tween in
	local blurIn = TweenService:Create(blur, TweenInfo.new(0.25, Enum.EasingStyle.Sine), {Size = MaxBlur})
	blurIn:Play()
	sound:Play()

	return blur, cc, sound
end

local function removeEffect(blur, cc, sound)
	local blurOut = TweenService:Create(blur, TweenInfo.new(0.4, Enum.EasingStyle.Sine), {Size = 0})
	local ccOut = TweenService:Create(cc, TweenInfo.new(0.4, Enum.EasingStyle.Sine), {Saturation = 0, Contrast = 1, TintColor = Color3.new(1,1,1)})

	blurOut:Play()
	ccOut:Play()
	blurOut.Completed:Wait()

	if blur then blur:Destroy() end
	if cc then cc:Destroy() end
	if sound then sound:Destroy() end
end

-- Input Handling
UIS.InputBegan:Connect(function(input, gpe)
	if gpe then return end
	if input.KeyCode ~= Enum.KeyCode.LeftShift then return end
	if activated then return end

	holding = true
	send("OpenWarp")
	local startTime = tick()

	task.spawn(function()
		task.wait(TapDuration)
		if holding and (tick() - startTime) < HoldTime then
			send("PulseWarp")
		end
	end)

	task.spawn(function()
		task.wait(HoldTime)
		if holding then
			-- Full Chronal Dash Trigger
			activated = true
			local blur, cc, sound = createEffect()

			-- Time freeze simulation (locally stop motion)
			hum.WalkSpeed = 0
			hum.JumpPower = 0

			task.wait(0.2) -- slow phase period before teleport
			hrp.CFrame = hrp.CFrame + hrp.CFrame.LookVector * TeleportDistance

			task.wait(0.3)
			removeEffect(blur, cc, sound)

			hum.WalkSpeed = 16
			hum.JumpPower = 50

			send("CloseWarp")
			activated = false
			holding = false
		end
	end)
end)

UIS.InputEnded:Connect(function(input, gpe)
	if gpe then return end
	if input.KeyCode ~= Enum.KeyCode.LeftShift then return end

	if not activated then
		-- Short tap (cancel)
		send("CloseWarp")
		holding = false
	end
end)
