local TweenService = game:GetService("TweenService")

local mask = script.Parent
local flailEvent = mask:WaitForChild("FlailJumpEvent")

-- References
local flailIndicator = mask:WaitForChild("FlailJumpIndicator")

-- Closed and open states
local ClosedSize = UDim2.new(0, 100, 0, 0)
local ClosedPos  = UDim2.new(0.574, 0, 0.921, 0)
local OpenedSize = UDim2.new(0, 100, 0, 100)
local OpenedPos  = UDim2.new(0.574, 0, 0.921, 0)

-- Force closed at start
mask.Size = ClosedSize
mask.Position = ClosedPos
mask.Visible = true

-- Indicator starts invisible
flailIndicator.ImageTransparency = 1

-- Tween settings
local ShellTween   = TweenInfo.new(0.2, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
local InsideFill   = TweenInfo.new(2, Enum.EasingStyle.Linear)
local InsideDrain  = TweenInfo.new(1, Enum.EasingStyle.Linear)
local IndicatorFadein  = TweenInfo.new(0.4, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
local IndicatorFadeout = TweenInfo.new(0.3, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)

-- State tracking
local fillingTween
local filled = false

-- Handlers
local function openShell()
	TweenService:Create(mask, ShellTween, {
		Size = OpenedSize,
		Position = OpenedPos
	}):Play()
end

local function closeShell()
	TweenService:Create(mask, ShellTween, {
		Size = ClosedSize,
		Position = ClosedPos
	}):Play()
end

local function fillInsides()
	if fillingTween then
		fillingTween:Cancel()
	end
	fillingTween = TweenService:Create(mask, InsideFill, {
		Size = OpenedSize,
		Position = OpenedPos
	})
	fillingTween:Play()

	fillingTween.Completed:Connect(function()
		if not filled then
			filled = true
			-- Fade in the indicator when full
			TweenService:Create(flailIndicator, IndicatorFadein, {ImageTransparency = 0}):Play()
		end
	end)
end

local function drainInsides()
	if fillingTween then
		fillingTween:Cancel()
	end
	filled = false
	TweenService:Create(mask, InsideDrain, {
		Size = ClosedSize,
		Position = ClosedPos
	}):Play()

	-- Fade out the indicator right after emptying
	TweenService:Create(flailIndicator, IndicatorFadeout, {ImageTransparency = 1}):Play()
end

-- Event Listening
flailEvent.Event:Connect(function(action)
	if action == "OpenShell" then
		openShell()
	elseif action == "CloseShell" then
		closeShell()
	elseif action == "FillInsides" then
		fillInsides()
	elseif action == "DrainInsides" then
		drainInsides()
	end
end)
