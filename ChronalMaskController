local TweenService = game:GetService("TweenService")
local frame = script.Parent
local event = frame:WaitForChild("ChronalDashEvent")

frame.Visible = false
frame.Size = UDim2.new(0, 100, 0, 0)
frame.Position = UDim2.new(0.401, 0, 0.923, 0)

-- Tween states
local closedSize = UDim2.new(0, 100, 0, 0)
local closedPos = UDim2.new(0.401, 0, 0.923, 0)
local openSize = UDim2.new(0, 100, 0, 112)
local openPos = UDim2.new(0.401, 0, 0.923, 0)

-- Helper for tween creation
local function tweenFrame(sizeGoal, posGoal, duration)
	local t1 = TweenService:Create(frame, TweenInfo.new(duration, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
		Size = sizeGoal,
		Position = posGoal
	})
	t1:Play()
	t1.Completed:Wait()
end

-- Bindable actions
event.Event:Connect(function(action)
	if action == "OpenWarp" then
		frame.Visible = true
		tweenFrame(openSize, openPos, 0.25)
	elseif action == "CloseWarp" then
		tweenFrame(closedSize, closedPos, 0.25)
		task.wait(0.25)
		frame.Visible = false
	elseif action == "PulseWarp" then
		-- Quick flicker pulse for charge
		local enlarge = TweenService:Create(frame, TweenInfo.new(0.1, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {
			Size = openSize + UDim2.new(0, 20, 0, 20)
		})
		local shrink = TweenService:Create(frame, TweenInfo.new(0.15, Enum.EasingStyle.Sine, Enum.EasingDirection.In), {
			Size = openSize
		})
		enlarge:Play()
		enlarge.Completed:Wait()
		shrink:Play()
	end
end)
