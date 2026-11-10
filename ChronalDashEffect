-- ChronalDashEffect (LocalScript inside ChronalMask)
local Lighting = game:GetService("Lighting")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local event = Instance.new("BindableEvent")
event.Name = "ChronalDashEffectEvent"
event.Parent = ReplicatedStorage

event.Event:Connect(function(player, action)
	if action == "Start" then
		local blur = Instance.new("BlurEffect")
		blur.Size = 0
		blur.Parent = Lighting
		local tweenIn = TweenService:Create(blur, TweenInfo.new(0.2), {Size = 24})
		tweenIn:Play()

		local atmosphere = Instance.new("ColorCorrectionEffect")
		atmosphere.Name = "ChronalAura"
		atmosphere.TintColor = Color3.fromRGB(180, 180, 255)
		atmosphere.Contrast = 0.7
		atmosphere.Saturation = -0.5
		atmosphere.Parent = Lighting

	elseif action == "End" then
		for _, eff in pairs(Lighting:GetChildren()) do
			if eff:IsA("BlurEffect") or eff.Name == "ChronalAura" then
				TweenService:Create(eff, TweenInfo.new(0.6), {Enabled = false}):Play()
				game.Debris:AddItem(eff, 1)
			end
		end
	end
end)
