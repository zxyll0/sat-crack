local dist = (CB.Character:FindFirstChild("HumanoidRootPart").Position - WR.Character:FindFirstChild("HumanoidRootPart").Position).magnitude
							if dist < MaxDistance then
								ClosestCB = CB
								MaxDistance = dist
							end
						end
					end
				end
			end
		else
			for index, CB in next, Players:GetPlayers() do
				if CB ~= WR and CB ~= Player and CB.Team ~= Player.Team then
					if CB:IsA("Player") and CB.Character then
						local dist = (CB.Character:FindFirstChild("HumanoidRootPart").Position - WR.Character:FindFirstChild("HumanoidRootPart").Position).magnitude
						if dist < MaxDistance then
							ClosestCB = CB
							MaxDistance = dist
						end
					end
				end
			end
		end
		return ClosestCB
	end

	local function Interceptable(Corner, LandingPos, t)
		if Corner and Corner.Character then
			local Dist = (Corner.Character:FindFirstChild("HumanoidRootPart").Position - LandingPos).magnitude
			
			local WalksSpeedOFPlr = Corner.Character:FindFirstChild("Humanoid").WalkSpeed
			local DiveingNumberAccountedFor = 0.31
			local eqq = (Dist / WalksSpeedOFPlr) - DiveingNumberAccountedFor --// minus some time because people can dive //--
			
			local Percent = (Dist * 0.70)
			local HighestThreshHold = (Dist - Percent)
			if eqq <= t then
				return true
			elseif Dist == 0 then
				return true
			else
				return false
			end
		end
	end


	local function getClosestCBtoBot(BotHere)
		local CbBot;
		for index, CBBot in next, workspace:GetChildren() do
			if game.PlaceId == 8206123457 and CBBot.Name == "npcwr" then
			local A = CBBot["a"]
			local B = CBBot["b"]
			local ACBBot = A["bot 2"]
			local BCBBot = B["bot 4"]
				if BotHere.Name == "bot 1" then
					CbBot = ACBBot
				elseif BotHere.Name == "bot 3" then
					CbBot = BCBBot
				end
			end  
		end
		return CbBot
	end

	local function botInterceptable(Corna, LandingEstimatedPos, t)
		if Corna:FindFirstChild("HumanoidRootPart") then
			local BotDist = (Corna:FindFirstChild("HumanoidRootPart").Position - LandingEstimatedPos).magnitude
			local WalksSpeedOFPlr = 20
			local DiveingNumberAccountedFor = 0.31
			local eqq = (BotDist / WalksSpeedOFPlr) - DiveingNumberAccountedFor --// minus some time because people can dive //--
			
			local Percenty = (BotDist * 0.70)
			local Highest = (BotDist - Percenty)
			if eqq <= t then
				return true
			elseif BotDist == 0 then
				return true
			else
				return false
			end
		end
	end

	local function CatchAble(wr, LandingPos, TimeOfProjectileFlight)
		if wr and wr.Character then
			local Dist = (wr.Character:FindFirstChild("HumanoidRootPart").Position - LandingPos).magnitude
			local WalksSpeedOFPlr = wr.Character:FindFirstChild("Humanoid").WalkSpeed
			local DiveingNumberAccountedFor = 0.31
			local eqq = (Dist / WalksSpeedOFPlr) - DiveingNumberAccountedFor --// minus some time because people can dive //--
			local Percent = (Dist * 0.70)
			local HighestThreshHold = (Dist - Percent)
			local WalkSpeed = 16
			if eqq <= TimeOfProjectileFlight then
				return true
			elseif Dist == 0 then
				return true
			else
				return false
			end
		end
	end
	
	local function botCatchAble(Wr, LandingEstimatedPoss)
		if Wr:FindFirstChild("HumanoidRootPart") then
			local BotDist = (Wr:FindFirstChild("HumanoidRootPart").Position - LandingEstimatedPoss).magnitude
			local Percenty = (BotDist * 0.70)
			local Highest = (BotDist - Percenty)
			if BotDist <= Highest then
				return true
			elseif BotDist == 0 then
				return true
			else
				return false
			end
		end
	end
	local function clampnum(val, minmimum, maxValue)
		return math.min(math.max(val, minmimum), maxValue)
	end


	-- // Keep the Throwing Position in the Bounds // --
	local function KeepPosInBounds(TargetPos, MinX, MinZ)
		local X = TargetPos.X
		local Y = TargetPos.Y
		local Z = TargetPos.Z
		local clampedX;
		local clampedZ;
		if TargetPos.X < -MinX then
			clampedX = -70.5
		elseif TargetPos.X > MinX then
			clampedX = 70.5
		elseif TargetPos.X > -MinX and TargetPos.X < MinX then
			clampedX = X
		end

		if TargetPos.Z < -MinZ then
			clampedZ = -175.5
		elseif TargetPos.Z > MinZ then
			clampedZ = 175.5
		elseif TargetPos.Z > -MinZ and TargetPos.Z < MinZ then
			clampedZ = Z
		end
		local ClampedVector3 = Vector3.new(clampedX, Y, clampedZ)
		return ClampedVector3
	end








	--// Round Number to Hundreths function //--
	local function RoundNumToHundredths(number)
		return math.floor(number * 100 + 0.5) / 100
	end

	
	local customLeadDistances = false
	local function GetTargetPositionForWR(Time, WideReceiver)
		if WideReceiver.Character and WideReceiver.Character:FindFirstChild("HumanoidRootPart") then
			local WRMovingVelocity = WideReceiver.Character:FindFirstChild("Humanoid").MoveDirection
			local WRMovingVelocity2 = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Velocity
			local TypeThrow = getThrowType()
			
			local LeadNumtab;

			local fieldOrientation = getFieldOrientation(Player.Character:FindFirstChild("HumanoidRootPart").Position, WideReceiver.Character:FindFirstChild("Humanoid").MoveDirection)
			if isMoving(WideReceiver) then
				if fieldOrientation == 1 then
					LeadNumtab = {
						["Dime"] = Vector3.new(1, 1.65, 9),   
						["Mag"] = Vector3.new(2, 2, 11),
						["Dive"] = Vector3.new(1.25, 1.86, 9.5),
						["Dot"] = Vector3.new(1, 1.2, 7),
						["Fade"] = Vector3.new(0, 0, 0),
						["Bullet"] = Vector3.new(5, 1, 1),
						["Jump"] = Vector3.new(1, 2.25, 7.5)
					}
				elseif fieldOrientation == -1 then
					LeadNumtab = {
						["Dime"] = Vector3.new(1, 1.65, -9),   
						["Mag"] = Vector3.new(2, 2, -11),
						["Dive"] = Vector3.new(1.25, 1.86, -9.5),
						["Dot"] = Vector3.new(1, 1.2, -7),
						["Fade"] = Vector3.new(0, 0, 0),
						["Bullet"] = Vector3.new(-5, 1, -1),
						["Jump"] = Vector3.new(1, 2.25, -7.5)
					}
				end
			else
				LeadNumtab = {
					["Dime"] = Vector3.new(0, 0, 0),   
					["Mag"] = Vector3.new(0, 0, 0),
					["Dive"] = Vector3.new(0, 0, 0),
					["Dot"] = Vector3.new(0, 0, 0),
					["Fade"] = Vector3.new(0, 0, 0),
					["Bullet"] = Vector3.new(0, 0, 0),
					["Jump"] = Vector3.new(0, 5, 0)
				}
			end
			local ThrowTypeAccountability;
			if Highestpwrmode then
				ThrowTypeAccountability = (WRMovingVelocity2 * Time)
			else
				if customLeads then
                    if TypeThrow == "Dot" then
                        ThrowTypeAccountability = (WRMovingVelocity * customLead * Time)
                    elseif TypeThrow == "Bullet" then
                        local XZAXIS = Vector3.new(WRMovingVelocity.X, 0, WRMovingVelocity.Z)
                        ThrowTypeAccountability = (XZAXIS * customLead * Time)	
                    elseif TypeThrow == "Jump" then
                        ThrowTypeAccountability = (WRMovingVelocity * customLead * Time)					
                    elseif TypeThrow == "Dime" then
                        ThrowTypeAccountability = (WRMovingVelocity * customLead * Time)		
                    elseif TypeThrow == "Dive" then
                        ThrowTypeAccountability = (WRMovingVelocity * customLead * Time)	
                    elseif TypeThrow == "Mag" then
                        ThrowTypeAccountability = (WRMovingVelocity * customLead * Time)	
                    end
                else
                    if TypeThrow == "Dot" then
                        ThrowTypeAccountability = (WRMovingVelocity * 17.5 * Time)
                    elseif TypeThrow == "Bullet" then
                        local XZAXIS = Vector3.new(WRMovingVelocity.X, 0, WRMovingVelocity.Z)
                        ThrowTypeAccountability = (XZAXIS * 21* Time)	
                    elseif TypeThrow == "Jump" then
                        ThrowTypeAccountability = (WRMovingVelocity * 18.5 * Time)					
                    elseif TypeThrow == "Dime" then
                        ThrowTypeAccountability = (WRMovingVelocity * 18.9 * Time)		
                    elseif TypeThrow == "Dive" then
                        ThrowTypeAccountability = (WRMovingVelocity * 19.3 * Time)	
                    elseif TypeThrow == "Mag" then
                        ThrowTypeAccountability = (WRMovingVelocity * 19.7 * Time)	
                    end
                end
			end
		
			
			local Equation
			if Highestpwrmode then
				if isMoving(WideReceiver) then
					if TypeThrow == "Fade" then
						Equation = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Position + LeadNumtab[TypeThrow]
					elseif TypeThrow == "Bullet" then
						Equation = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Position + (ThrowTypeAccountability) + LeadNumtab[TypeThrow]
					else
						Equation = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Position + (ThrowTypeAccountability) + LeadNumtab[TypeThrow]
					end
				elseif not isMoving(WideReceiver) and TypeThrow == "Jump" then --// always make it a jump throw even if not moving //--
					Equation = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Position + (ThrowTypeAccountability) + Vector3.new(0, 6, 0)
				else
					Equation = WideReceiver.Character:FindFirstChild("HumanoidRootPart").Position
				end
			else
				if isMoving(WideReceiver) then
					
						Equation = WideReceiver.Character.Head.Position + (ThrowTypeAccountability) + LeadNumtab[TypeThrow]
				
				elseif not isMoving(WideReceiver) and TypeThrow == "Jump" then --// always make it a jump throw even if not moving //--
					Equation = WideReceiver.Character.Head.Position + (ThrowTypeAccountability) + Vector3.new(0, 6, 0)
				else
					Equation = WideReceiver.Character.Head.Position 
				end
			end
   

			return Equation
		else
			warn("Wide Receiver or HumanoidRootPart not found")
			return Vector3.new(0, 0, 0)
		end
	end


	local Data = {
		Direction = Vector3.new(0, 0, 0),
		NormalPower = 55,		
		BulletModeAngle = 5,
		FadeModeAngle = 55,
		LowestPower = 40,
		MaxPower = 95,
		Angle = 45,
		MaxAngle = 55,
		lowestAngle = 10
	}

	
	--// Function to predict the projectile landing position //--
	local function predicitLand(Velocity, Gravity, num, start, powa, timeoflight)
		local Vel = powa * Velocity
		local position = start + Vel * timeoflight + 0.5 * Gravity * timeoflight * timeoflight
		  return position
	end

	--// Adjust Angle Manually Connection//--
	game:GetService("UserInputService").InputBegan:Connect(function(input, typeing)
		if not AutoAngie and not typeing then
			local TT = getThrowType()
			if TT == "Bullet" then
				if input.KeyCode == Enum.KeyCode.R and Data.BulletModeAngle < 20 then
					Data.BulletModeAngle = Data.BulletModeAngle + 5
				elseif input.KeyCode == Enum.KeyCode.F and Data.BulletModeAngle > 5 then
					Data.BulletModeAngle = Data.BulletModeAngle - 5
				elseif input.KeyCode == Enum.KeyCode.R and Data.BulletModeAngle == 20 then                                        
					warn("Cannot Up Angle Any more, Max Angle is 20")
					Data.BulletModeAngle = Data.BulletModeAngle + 0
				elseif input.KeyCode == Enum.KeyCode.F and Data.BulletModeAngle == 5 then
					warn("Cannot Lower Angle Any more, Lowest Angle is 5")
					Data.BulletModeAngle = Data.BulletModeAngle - 0
				end
			elseif TT == "Fade" then
				if input.KeyCode == Enum.KeyCode.R and Data.FadeModeAngle < 75 then
					Data.FadeModeAngle = Data.FadeModeAngle + 5
				elseif input.KeyCode == Enum.KeyCode.F and Data.FadeModeAngle > 55 then
					Data.FadeModeAngle = Data.FadeModeAngle - 5
				elseif input.KeyCode == Enum.KeyCode.R and Data.FadeModeAngle == 75 then                                        
					warn("Cannot Up Angle Any more, Max Angle is 75")
					Data.FadeModeAngle = Data.FadeModeAngle + 0
				elseif input.KeyCode == Enum.KeyCode.F and Data.FadeModeAngle == 55 then
					warn("Cannot Lower Angle Any more, Lowest Angle is 55")
					Data.FadeModeAngle = Data.FadeModeAngle - 0
				end
			else
				if input.KeyCode == Enum.KeyCode.R and Data.Angle < 55 then
					Data.Angle = Data.Angle + 5
				elseif input.KeyCode == Enum.KeyCode.F and Data.Angle > 10 then
					Data.Angle = Data.Angle - 5
				elseif input.KeyCode == Enum.KeyCode.R and Data.Angle == 55 then                                        
					warn("Cannot Up Angle Any more, Max Angle is 55")
					Data.Angle = Data.Angle + 0
				elseif input.KeyCode == Enum.KeyCode.F and Data.Angle == 10 then
					warn("Cannot Lower Angle Any more, Lowest Angle is 10")
					Data.Angle = Data.Angle - 0
				end
			end
		end
	end)

	--// Adjust Power Manually Connection//--
	game:GetService("UserInputService").InputBegan:Connect(function(input, typein)
		if not AutoPowa and not typein then
			if input.KeyCode == Enum.KeyCode.Z and Data.NormalPower < Data.MaxPower then
				Data.NormalPower = Data.NormalPower + 5
			elseif input.KeyCode == Enum.KeyCode.X and Data.NormalPower > Data.LowestPower then
				Data.NormalPower = Data.NormalPower - 5
			elseif input.KeyCode == Enum.KeyCode.Z and Data.NormalPower == Data.MaxPower then
				Data.NormalPower = Data.NormalPower + 0
				warn("Max Power, Cannot Adjust Any Higher")	
			elseif input.KeyCode == Enum.KeyCode.X and Data.NormalPower == Data.LowestPower then
				Data.NormalPower = Data.NormalPower - 0
				warn("Lowest Possible Power, Cannot Adjust Any Lower")										
			end
		end
	end)
	-------/------/------/---/-------/----------/-----/------/-------------/-----------/--------------/----------/---------
	local function isVector3Valid(vec3)
		return not (vec3.X ~= vec3.X or vec3.Y ~= vec3.Y or vec3.Z ~= vec3.Z)
	end
	
	local ThrowingTab = {
		Direction = Vector3.new(0, 0, 0)
	}
	local throwingpar = Instance.new("Part")
	

								throwingpar.Size = Vector3.new(1, 1, 1)
								throwingpar.Color = Color3.fromRGB(0, 0, 0)
								throwingpar.Anchored = false
	game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessedEvent)
		if game.PlaceId ~= 8206123457 then
			if input.UserInputType == Enum.UserInputType.MouseButton1 and game:GetService("Players").LocalPlayer.PlayerGui.MainGui.Message.Text == "HIKE" and game:GetService("ReplicatedStorage").Values.Status.Value == "InPlay" and game:GetService("ReplicatedStorage").Values.Throwable and not gameProcessedEvent then
					if Char then
						local Football = Char:FindFirstChildOfClass("Tool")
						if Football then
							if state == true then
								if state == false then return end
								local start = Player.Character:FindFirstChild("Head").Position
								if not isLocked then
									if autoswr then
										ClosestPlr = getMostIsolatedPlayer(dradius)
									else
										local nearestPlayer = getNearestPlayerToMouse()
										if nearestPlayer and nearestPlayer:IsA("Player") then
											ClosestPlr = nearestPlayer
										end
									end
								end
								
								if isLocked and not ClosestPlr then
									if autoswr then
										if getMostIsolatedPlayer(dradius) == nil then
											ClosestPlr = ClosestPlr
										else
											ClosestPlr = getMostIsolatedPlayer(dradius)
										end
									else
										if getNearestPlayerToMouse() == nil then
											ClosestPlr = ClosestPlr
										else
											ClosestPlr = getNearestPlayerToMouse()
										end 
									end
								end
								local Initial = 95
								local Throwtype;
									
							
									Throwtype = getThrowType()
							
								local vel;
								local toThrowToDirection;
								local pow;


								local WhichOne2;
								if Throwtype == "Fade" then
									WhichOne2 = Data.FadeModeAngle
								elseif Throwtype == "Bullet" then
									WhichOne2 = Data.BulletModeAngle
								else
									WhichOne2 = Data.Angle
								end

								if Highestpwrmode then
									Initial = 95
								else
									if AutoPowa then
										if pow then
										Initial = pow
									else
										 Initial = 95
									end
								else
									Initial = Data.NormalPower
								end
							end
		
								local toLaunchAnlge;
								if Highestpwrmode then
									if AutoAngie then
										toLaunchAnlge = HighSpeedLowAngleCalcs(FF2Grav, Initial)
									else
										toLaunchAnlge = math.rad(WhichOne2)
									end
								else
									if AutoAngie then
										if Throwtype == "Fade" then
											toLaunchAnlge = math.rad(75)
										elseif Throwtype == "Bullet" then
											toLaunchAnlge = clampnum(HighSpeedLowAngleCalcs(FF2Grav, Initial), 0, 0.296706)
										else
											toLaunchAnlge = clampnum(calculateLaunchAngle(FF2Grav, Initial), 0, 0.975)
										end
									else
										toLaunchAnlge = math.rad(WhichOne2)
									end
								end
								local TOF = GetTimeOfFlightProjectile(Initial, toLaunchAnlge, FF2Grav)
								local YesEnd;
								if string.find(ClosestPlr.name, "bot 1") or string.find(ClosestPlr.name, "bot 3") then
									YesEnd = KeepPosInBounds(BotEstimatedVel(TOF, ClosestPlr), 70.5, 175.5)
								elseif not string.find(ClosestPlr.name, "bot 1") or not string.find(ClosestPlr.name, "bot 3") then
									YesEnd = KeepPosInBounds(GetTargetPositionForWR(TOF, ClosestPlr), 70.5, 175.5)
								end
								local PowerSir;
								 vel, toThrowToDirection, pow = OverallVelocityNeededToReachAPosition(toLaunchAnlge, start, YesEnd, Vector3.new(0,-FF2Grav,0), TOF)  
								if AutoPowa then
									if Throwtype == "Fade" then
										PowerSir = 95
									elseif Throwtype == "Bullet" then
										PowerSir = clampnum(pow, 90, 95)
									else
										PowerSir = pow
									end
								else
									PowerSir = Data.NormalPower
								end
								local neworigin = start + (ThrowingTab.Direction * 5)

								local RemoteEvent = Football.Handle:FindFirstChild("RemoteEvent")
								if RemoteEvent then
									local ThrowAnimation = Char.Humanoid:LoadAnimation(game:GetService("ReplicatedStorage").Animations.Throw)
									ThrowAnimation.Name = "Throw"
									ThrowAnimation:Play()
									RemoteEvent:fireServer("Clicked", start, neworigin + ThrowingTab.Direction * 10000, (game.PlaceId == 8206123457 and PowerSir) or 95, PowerSir)
									
								else
							   
								end 
							else

							end
						else
		   
						end
					else
				   
					end
				end
			elseif game.PlaceId == 8206123457 then
				if input.UserInputType == Enum.UserInputType.MouseButton1 and not gameProcessedEvent then
					if Char then
						local Football = Char:FindFirstChildOfClass("Tool")
						if Football then
							if state == true then
								if state == false then return end
								local start = Player.Character:FindFirstChild("Head").Position
								if not isLocked then
									if autoswr then
										ClosestPlr = getMostIsolatedPlayer(dradius)
									else
										local nearestPlayer = getNearestPlayerToMouse()
										if nearestPlayer and nearestPlayer:IsA("Player") then
											ClosestPlr = nearestPlayer
										end
									end
								end
								
								if isLocked and not ClosestPlr then
									if autoswr then
										if getMostIsolatedPlayer(dradius) == nil then
											ClosestPlr = ClosestPlr
										else
											ClosestPlr = getMostIsolatedPlayer(20)
										end
									else
										if getNearestPlayerToMouse() == nil then
											ClosestPlr = ClosestPlr
										else
											ClosestPlr = getNearestPlayerToMouse()
										end 
									end
								end
								local Initial = 95
								local Throwtype;
                                if autopmode then
                                    Throwtype = calculateThrowType(ClosestPlr)
                                else
                                    Throwtype = getThrowType()
                                end
								
							  
								local vel;
								local toThrowToDirection;
								local pow;

								local WhichOne2;
								if Throwtype == "Fade" then
									WhichOne2 = Data.FadeModeAngle
								elseif Throwtype == "Bullet" then
									WhichOne2 = Data.BulletModeAngle
								else
									WhichOne2 = Data.Angle
								end
								if Highestpwrmode then
									Initial = 95
								else
									if AutoPowa then
										if pow then
										Initial = pow
									else
										 Initial = 95
									end
								else
									Initial = Data.NormalPower
								end
							end
								local toLaunchAnlge;
								if Highestpwrmode then
									if AutoAngie then
										toLaunchAnlge = HighSpeedLowAngleCalcs(FF2Grav, Initial)
									else
										toLaunchAnlge = math.rad(WhichOne2)
									end
								else
									if AutoAngie then
										if Throwtype == "Fade" then
											toLaunchAnlge = math.rad(75)
										elseif Throwtype == "Bullet" then
											toLaunchAnlge = clampnum(HighSpeedLowAngleCalcs(FF2Grav, Initial), 0, 0.296706)
										else
											toLaunchAnlge = clampnum(calculateLaunchAngle(FF2Grav, Initial), 0, 0.975)
										end
									else
										toLaunchAnlge = math.rad(WhichOne2)
									end
								end
								local TOF = GetTimeOfFlightProjectile(Initial, toLaunchAnlge, FF2Grav)
								local YesEnd;
								if string.find(ClosestPlr.name, "bot 1") or string.find(ClosestPlr.name, "bot 3") then
										YesEnd = BotEstimatedVel(TOF, ClosestPlr)
								elseif not string.find(ClosestPlr.name, "bot 1") or not string.find(ClosestPlr.name, "bot 3") then
										YesEnd = GetTargetPositionForWR(TOF, ClosestPlr)
								end
								local PowerSir;

								vel, toThrowToDirection, pow = OverallVelocityNeededToReachAPosition(toLaunchAnlge, start, YesEnd, Vector3.new(0,-FF2Grav,0), TOF)  
								if AutoPowa then
									if Throwtype == "Fade" then
										PowerSir = 95
									elseif Throwtype == "Bullet" then
										PowerSir = clampnum(pow, 90, 95)
									else
										PowerSir = pow
									end
								else
									PowerSir = Data.NormalPower
								end
								local neworigin = start + (ThrowingTab.Direction * 5)

								local RemoteEvent = Football.Handle:FindFirstChild("RemoteEvent")
								if RemoteEvent then
									local ThrowAnimation = Char.Humanoid:LoadAnimation(game:GetService("ReplicatedStorage").Animations.Throw)
									ThrowAnimation.Name = "Throw"
									ThrowAnimation:Play()
									RemoteEvent:fireServer("Clicked", start, neworigin + ThrowingTab.Direction * 10000, (game.PlaceId == 8206123457 and PowerSir) or 95, PowerSir)
								else
									
								end 
							else
							
							end
						else
						  
						end
					else
				  
					end
				end
			end
		end)


		
	local TargetPosition;
	local PredictedRoute
	
	--// Connection to make it Click to Throw //--
	Char.ChildAdded:Connect(function(v)
		if v.Name == "Football" and Char then
			local children = v:GetChildren()
			if children.Name == "Handle" then
				local descendants = children:GetChildren()
				if descendants.Name == "LocalScript" then
					descendants:Destroy()
				end
			end
		end
	end)
	
	local customBeam = false
	--// One big function that holds function for if conditions //--
	task.spawn(function()
		game:GetService('RunService').Heartbeat:Connect(function()
			task.wait()
			
			if not isLocked then
				if autoswr then
                    ClosestPlr = getMostIsolatedPlayer(dradius)
                else
                    ClosestPlr = getNearestPlayerToMouse()
                end
			end
			
			
			local PredictedRoute;

			
			task.wait()
			if state  then
				if Player.Character and Player.Character:FindFirstChild("Football") and ClosestPlr then
					beam.Enabled = true
				
				local Throwtype;
				if autopmode then
                    Throwtype = calculateThrowType(ClosestPlr)
                else
                    Throwtype = getThrowType()
                end
				
			  
				
				Highlight.Enabled = true
				Highlight.OutlineTransparency = 0
				Highlight.FillColor = Color3.new(0.34117647058, 0.34117647058, 1)
				Highlight.OutlineColor = Color3.new(0.803921, 0.898039, 0.941176)
				--[[if not string.find(ClosestPlr.Name, "bot 1") and not string.find(ClosestPlr.Name, "bot 3") then
					PredictedRoute = CalculateRouteofPlayer(ClosestPlr)
				elseif string.find(ClosestPlr.Name, "bot 1") or  string.find(ClosestPlr.Name, "bot 3") then
					PredictedRoute = "Straight"
				end--]]


				if not string.find(ClosestPlr.Name, "bot 1") and not string.find(ClosestPlr.Name, "bot 3") then
					if ClosestPlr.Character:FindFirstChild("HumanoidRootPart") then
						Highlight.Parent = ClosestPlr.Character
						
					end
				elseif string.find(ClosestPlr.Name, "bot 1") or string.find(ClosestPlr.Name, "bot 3") then
					Highlight.Parent = ClosestPlr	
					
				end
				ScreenGui.Enabled = true
				
				local WhichOne;
				if Throwtype == "Fade" then
					WhichOne = Data.FadeModeAngle
				elseif Throwtype == "Bullet" then
					WhichOne = Data.BulletModeAngle
				else
					WhichOne = Data.Angle
				end


				local FF2Grav = 28
				local Start = Player.Character:FindFirstChild("Head").Position
				local power;
				local velocity;
				local direction;
				local Initial;
				local LaunchAngle;
				if Highestpwrmode then
						Initial = 95
				else
					if AutoPowa then
						if power then
							Initial = power
						else
							Initial = 95
						end
					else
						Initial = Data.NormalPower
					end

				end

				if Highestpwrmode then
					if AutoAngie then
						LaunchAngle = HighSpeedLowAngleCalcs(FF2Grav, Initial)
					else
						LaunchAngle = math.rad(WhichOne)
					end
				else
					if AutoAngie then
						if Throwtype == "Fade" then
							LaunchAngle = math.rad(75)
						elseif Throwtype == "Bullet" then
							LaunchAngle = clampnum(HighSpeedLowAngleCalcs(FF2Grav, Initial), 0, 0.296706)
						else
							LaunchAngle = clampnum(calculateLaunchAngle(FF2Grav, Initial), 0, 0.975)
						end
					else
						LaunchAngle = math.rad(WhichOne)
					end
				end

				
				local TOF = GetTimeOfFlightProjectile(Initial, LaunchAngle, FF2Grav)
				local TargetPosition;
				
				if (string.find(ClosestPlr.Name, "bot 1") or string.find(ClosestPlr.Name, "bot 3")) then
					if game.PlaceId == 8206123457 then
						TargetPosition = BotEstimatedVel(TOF, ClosestPlr)
					elseif game.PlaceId ~= 8206123457 then
						TargetPosition = KeepPosInBounds(BotEstimatedVel(TOF, ClosestPlr), 70.5, 175.5)
					end
				else
					if game.PlaceId == 8206123457 then
						TargetPosition = GetTargetPositionForWR(TOF, ClosestPlr)
					elseif game.PlaceId ~= 8206123457 then
						TargetPosition = KeepPosInBounds(GetTargetPositionForWR(TOF, ClosestPlr), 70.5, 175.5)
					end
				end
				
				local POWAA;
				
				 velocity, direction, power = OverallVelocityNeededToReachAPosition(LaunchAngle, Start, TargetPosition, Vector3.new(0, -FF2Grav, 0), TOF)
					if power and Initial == 95 and AutoPowa then
						Initial = power
					else
						Initial = Data.NormalPower
					end								
					
				if AutoPowa then
					if Throwtype == "Fade" then
						POWAA = 95
					elseif Throwtype == "Bullet" then
						POWAA = clampnum(power, 90, 95)
					else
						POWAA = power
					end
				else
					POWAA = Data.NormalPower
				end         
				if isVector3Valid(direction) and isVector3Valid(TargetPosition) then
					ThrowingTab.Direction = direction
					
					local startAdjusted = Start + (ThrowingTab.Direction * 5) -- // this is the beginning offsets on the server // --
					local ATime;
					if customBeam then
						ATimee = 10
					else
						ATimee = TOF
					end
					local curve0, curve1, cf0, cf1 = beamProjectile(Vector3.new(0, -FF2Grav, 0), POWAA * ThrowingTab.Direction, Start + (ThrowingTab.Direction * 5), ATimee)
					
					beam.CurveSize0 = curve0
					beam.CurveSize1 = curve1
					beam.Attachment0.CFrame = beam.Attachment0.Parent.CFrame:inverse() * cf0
					beam.Attachment1.CFrame = beam.Attachment1.Parent.CFrame:inverse() * cf1
					local PeakPos = ProjectilePeakPosition(startAdjusted, velocity, Vector3.new(0, -28, 0))
					throwingpar.Parent = workspace
					throwingpar.CFrame = CFrame.new(PeakPos)
					throwingpar.Anchored = true
					
					---// get beam rotation //--
					local sum = (beam.Attachment1.CFrame - beam.Attachment1.Position):Inverse()
					if endPartOfBeam == false then
						VisPart.CFrame = beam.Attachment1.CFrame * sum * CFrame.Angles(math.rad(0), 0, 0)
					end
					--trace.From = game:GetService("UserInputService"):GetMouseLocation()--
					local CamPo, OnScren = isVisandPos(VisPart.Position)
					local CamPo2, OnS = isVisandPos(beam.Attachment0.Position)
			
					TargetPlr.Text = ClosestPlr.Name
					PowerNum.Text = tostring(POWAA)
					
					
					
					if not (string.find(ClosestPlr.Name, "bot 1") or string.find(ClosestPlr.Name, "bot 3")) then
						local ClosestCB = getPeopleGuardingClosestToMouse(ClosestPlr)
						if Interceptable(ClosestCB, VisPart.Position, TOF) then
							IntText.Text = "Yes"
						else
							IntText.Text = "No"
						end
					elseif string.find(ClosestPlr.Name, "bot 1") or string.find(ClosestPlr.Name, "bot 3") then
						local BotCbClosest = getClosestCBtoBot(ClosestPlr)
						if botInterceptable(BotCbClosest, VisPart.Position, TOF) then
							IntText.Text = "Yes"
						else
							IntText.Text = "No"
						end
					end


					if not string.find(ClosestPlr.Name, "bot 1") and not string.find(ClosestPlr.Name, "bot 3") then
						local ClosestWRR = getNearestPlayerToMouse()
						if CatchAble(ClosestWRR, VisPart.Position, TOF) then
							CatchText.Text = "Yes"
						else
							CatchText.Text = "No"
						end
					elseif string.find(ClosestPlr.Name, "bot 1") or string.find(ClosestPlr.Name, "bot 3") then
						local BotCbWr = getNearestPlayerToMouse()
						if botCatchAble(BotCbWr, VisPart.Position) then
							CatchText.Text = "Yes"
						else
							CatchText.Text = "No"
						end
					end
				
                    airtimetext.Text = tostring(RoundNumToHundredths(TOF)) .. "s"
					
					
					if AutoAngie then
						if Throwtype == "Fade" then
							AngleNum.Text = "75"
						else
							AngleNum.Text = tostring(RoundNumToHundredths(math.deg(LaunchAngle)))
						end
					else
						AngleNum.Text = tostring(WhichOne)
					end
				end
			else
				ScreenGui.Enabled = false
				Highlight.Enabled = false

			end
		else
			beam.Enabled = false
			ScreenGui.Enabled = false
			Highlight.Enabled = false
		end
		end)
	end)
	local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local aRushOn = false
if game.PlaceId ~= 8206123457 then
	local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer
local charplr = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local hrp2 = charplr:FindFirstChild("HumanoidRootPart")
local hum2 = charplr:FindFirstChild("Humanoid")
local agDist = 20
LocalPlayer.CharacterAdded:Connect(function(character)
    charplr = character
end)

local function doGuarding()
	local BallCarrier = ReplicatedStorage.Values.QB.Value
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer and player.Team ~= LocalPlayer.Team and player == BallCarrier then
			print(BallCarrier)
            local car = player.Character
            if car  then
                local hrp = car:FindFirstChild("HumanoidRootPart")
                local hum = car:FindFirstChild("Humanoid")
                if hrp and hrp2 and hum and hum2 then
                    local WS = 20
                    local distance = (hrp.Position - hrp2.Position).magnitude
                    local TimeToGet = distance / WS
                    if distance <= agDist then
                        local equation = hrp.Position + (hum.MoveDirection * TimeToGet * WS)
                        hum2:MoveTo(equation)
                    end
                end
            end
        end
    end
end

task.spawn(function()
    while task.wait() do
        if aRushOn then
            doGuarding()
        end
    end
end)
end
	local pvon = false
    local pvstrength = 10
    local pvdist = 20
    local plr = game:GetService("Players").LocalPlayer
    local char = plr.Character
    plr.CharacterAdded:Connect(function(character)
        char = character
    end)
    
    task.spawn(function()
        workspace.ChildAdded:Connect(function(child)
            while task.wait() do
                if child.Name == "Football" and pvon and child:IsA("BasePart") then
                    local hrp = char:FindFirstChild("HumanoidRootPart")
                    local direction = (child.Position - hrp.Position).Unit
                    local dist = (child.Position - hrp.Position).magnitude


                        if dist <= pvdist then
                       
                            
                               
                                hrp.Velocity = direction * pvstrength
                                
                           
                        end
                    end
                end
        end)    
    end)
	getgenv().antiJamOn = false
task.spawn(function()
    while wait() do
        for index, player in pairs(game:GetService("Players"):GetChildren()) do
            if player ~= LocalPlayer then
                local char = player.Character
                if char then
                    for index, part in pairs(char:GetChildren()) do
                        if part:IsA("BasePart") or part:IsA("MeshPart") and part.CanCollide then
                            if antiJamOn then
                                part.CanCollide = false
                            else
                                part.CanCollide = true
                            end
                        end
                    end
                end
            end
        end
    end
end)
	local plrr = game:GetService("Players").LocalPlayer
    local charr = plr.Character
    plrr.CharacterAdded:Connect(function(character)
        charr = character
    end)
    local clicktpfon = false
	local clicktpstreng = 3
	uis.InputBegan:Connect(function(input, gameProcessedEvent)
		if input.KeyCode == Enum.KeyCode.F and clicktpfon then
			local hrp = charr:FindFirstChild("HumanoidRootPart")

			hrp.CFrame = hrp.CFrame + (hrp.CFrame.LookVector * clicktpstreng)

		end
	end)


    local angleEnhance;
	local Players = game:GetService("Players")
    local lp = Players.LocalPlayer
    local char = lp.Character
    lp.CharacterAdded:Connect(function(character)
        char = character
    end)
    local function onToggle2(Value)
        angleEnhance = Value
        if angleEnhance then
            connection = game:GetService("RunService").RenderStepped:Connect(function()
                local upWard = Vector3.new(0, 10, 0)
                local lp = Players.LocalPlayer
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                local hum = char and char:FindFirstChild("Humanoid")
                local AngleNumBa = (52.5 / 10)
                if hrp and hum and hum.FloorMaterial == Enum.Material.Grass and hum.Jump then
                    hrp.Velocity = upWard * AngleNumBa
                end
            end)
        else
            if connection then
                connection:Disconnect()
                connection = nil
            end
        end
    end
local BeOn = false
	workspace.ChildAdded:Connect(function(v)
		if v.Name == "Football" and v:IsA("BasePart") and BeOn then
				task.wait()
				local startTime = tick()
				local Beginning = v.Position
				local Vel3 = v.Velocity
				local t = 10
				local ff2G = Vector3.new(0, -28, 0)
				local curve0, curve1, cf1, cf2 = beamProjectile(ff2G, Vel3, Beginning, t)
				local beam = Instance.new("Beam")
				local Attach0 = Instance.new("Attachment", workspace.Terrain)
				local Attach1 = Instance.new("Attachment", workspace.Terrain)
		
				beam.Color = ColorSequence.new({
					ColorSequenceKeypoint.new(0, Color3.fromRGB(249, 12, 12)),
					ColorSequenceKeypoint.new(1, Color3.fromRGB(249, 12, 12))
				})
		
				beam.Parent = workspace.Terrain
				beam.CurveSize0 = curve0
				beam.CurveSize1 = curve1
				beam.Segments = 1750
		
				Attach0.CFrame = Attach0.Parent.CFrame:inverse() * cf1
				Attach1.CFrame = Attach1.Parent.CFrame:inverse() * cf2
				beam.Attachment0 = Attach0
				beam.Attachment1 = Attach1
		
				beam.Width0 = 0.5
				beam.Width1 = 0.5
				repeat 
					task.wait()
				until v.Parent ~= workspace
					Attach0:Destroy()
					Attach1:Destroy()
			end
		end)

		local OceanLib = Ocean:NewWindow()
		local t3 = OceanLib:Tab("QB", 10723426986)
		local t4 = OceanLib:Tab("QB Configs", 10723426986)
		local t1 = OceanLib:Tab("Catching", 10723426986)
		local t5 = OceanLib:Tab("Automatics", 10723426986)
		local t7 = OceanLib:Tab("Kicker", 10723426986)
		local t6 = OceanLib:Tab("Visuals", 10723426986)
		local t2 = OceanLib:Tab("Player", 10723424505)
		t1:Dropdown("Magnet Version", {
			Default  = "None",
			Options  = {"Magnets V1", "Magnets V2 (RISKY)"},
			Callback = function(v)
				msVersion = v
			end,
		})
		t1:Toggle("Magnets", {
			Default  = false,
			Callback = function(v)
				on = v
			end,
		})
		t1:Dropdown("Magnet Type", {
			Default  = "None",
			Options  = {"Blatant", "Legit", "Regular", "Leauge"},
			Callback = function(v)
				magType = v
			end,
		})
		t1:Slider("Blatant Mag Range", {
			Default  = 20,
			Min		 = 20,
			Max		 = 30,
			Callback = function(v)
				bldist = v
			end,
		})
		t1:Slider("Regular Mag Range", {
			Default  = 15,
			Min		 = 15,
			Max		 = 25,
			Callback = function(v)
				regdist = v
			end,
		})
		t1:Slider("Legit Mag Range", {
			Default  = 5,
			Min		 = 5,
			Max		 = 10,
			Callback = function(v)
				legmagdist = v
			end,
		})
		t1:Slider("Leauge Mag Range", {
			Default  = 0,
			Min		 = 0,
			Max		 = 5,
			Callback = function(v)
				leaugdist = v
			end,
		})
		t1:Slider("Magnets V2 Range", {
			Default  = 20,
			Min		 = 0,
			Max		 = 30,
			Callback = function(v)
				msSecondVerRange = v
			end,
		})
		t1:Toggle("Pull Vector", {
			Default  = false,
			Callback = function(v)
				pvon = v
			end,
		})
		t1:Slider("Pull Vector Strength", {
			Default  = 1,
			Min		 = 1,
			Max		 = 30,
			Callback = function(v)
				pvstrength = v
			end,
		})
		t1:Slider("Pull Vector Distance", {
			Default  = 1,
			Min		 = 1,
			Max		 = 30,
			Callback = function(v)
				pvdist = v
			end,
		})
		
		getgenv().VIM = game:GetService("VirtualInputManager")
getgenv().plrrrr = game:GetService("Players").LocalPlayer
getgenv().charrrr = plrrrr.Character
getgenv().kickOn = false
plrrrr.CharacterAdded:Connect(function(character)
    charrrr = character
end)
local autoCapOn = false
if game.PlaceId ~= 8206123457 then
	local endCaptainLine =   workspace:FindFirstChild("Models"):FindFirstChild("LockerRoomA"):FindFirstChild("FinishLine")
	local plr = game:GetService("Players").LocalPlayer
	local char = plr.Character or plr.CharacterAdded:Wait()
	local hrp = char:FindFirstChild("HumanoidRootPart")
	local autoCapOffset = Vector3.new(2.1, 2.1, 2.1)

	endCaptainLine:GetPropertyChangedSignal("Position"):Connect(function()
		if autoCapOn then
			task.wait(0.13)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
			task.wait(0.25)
			hrp.CFrame = endCaptainLine.CFrame + autoCapOffset
		end
	end)
end



game:GetService("Players").LocalPlayer.PlayerGui.ChildAdded:Connect(function(child)
	
local hum = charrrr:FindFirstChild("Humanoid")
    if child.Name == "KickerGui" and charrrr and hum and kickOn then
        local cursor = child:FindFirstChild("Cursor", true)
        local firstdone = false
        local seconddone = false
        VIM:SendMouseButtonEvent(0, 0, 0, true, nil, 0)
        repeat 
            task.wait()
        until cursor.Position.Y.Scale <= 0.03
        VIM:SendMouseButtonEvent(0, 0, 0, true, nil, 0)
        firstdone = true
        if firstdone then
            repeat 
                task.wait()
            until cursor.Position.Y.Scale >= 0.89
        end
       
            VIM:SendMouseButtonEvent(0, 0, 0, true, nil, 0)
            seconddone = true
          if seconddone and firstdone then
            VIM:SendMouseButtonEvent(0, 0, 0, true, nil, 0)
            hum:MoveTo(game.Workspace["KickerBall"].Position)
          end
    end
end)
t7:Toggle("Perfect Kicker Aimbot", {
	Default  = false,
	Callback = function(v)
		kickOn = v
	end,
})


getgenv().antiBlockOn = false

local PLR = game:GetService("Players").LocalPlayer
local charppp = PLR.Character
PLR.CharacterAdded:Connect(function(gg)
	charppp = gg
end)
local isAMatch = string.match
charppp.DescendantAdded:Connect(function(child)
	if isAMatch(child.Name, "FFmover") and antiBlockOn then
		print("Removed.")
		child:Destroy()
	end
end)
		
		local VIM = game:GetService("VirtualInputManager")
local PLR = game:GetService("Players").LocalPlayer
local charp = PLR.Character
local autoCatchOn = false
PLR.CharacterAdded:Connect(function(gg)
	charp = gg
end)

_G.dist = 17  

local function Autocatch()
    VIM:SendMouseButtonEvent(0, 0, 0, true, nil, 0)
    VIM:SendMouseButtonEvent(0, 0, 0, false, nil, 0)
end

game:GetService("RunService").RenderStepped:Connect(function()
    for index, ball in pairs(workspace:GetChildren()) do
        if ball.Name == "Football" and ball:IsA("BasePart") then
            if charp:FindFirstChild("HumanoidRootPart") and (charp:FindFirstChild("HumanoidRootPart").Position - ball.Position).Magnitude <= _G.dist and autoCatchOn then
                Autocatch()
            end
        end
    end
end)


getgenv().VIM = game:GetService("VirtualInputManager")
getgenv().PLR = game:GetService("Players").LocalPlayer
getgenv().charr = PLR.Character
getgenv().autoJumpOn = false
PLR.CharacterAdded:Connect(function(gg)
	charr = gg
end)

getgenv().disttt = 17  

function AutoJump()
    VIM:SendKeyEvent(true, Enum.KeyCode.Space, false, game)
    wait(0.1)
    VIM:SendKeyEvent(false, Enum.KeyCode.Space, false, game)
end

game:GetService("RunService").RenderStepped:Connect(function()
    for index, ball in pairs(workspace:GetChildren()) do
        if ball.Name == "Football" and ball:IsA("BasePart") then
            if charr:FindFirstChild("HumanoidRootPart") and (charr:FindFirstChild("HumanoidRootPart").Position - ball.Position).Magnitude <= disttt and autoJumpOn then
                AutoJump()
            end
        end
    end
end)
getgenv().VIM = game:GetService("VirtualInputManager")
getgenv().PLR = game:GetService("Players").LocalPlayer
getgenv().charrr = PLR.Character
getgenv().autoSwatOn = false
PLR.CharacterAdded:Connect(function(gg)
	charrr = gg
end)

getgenv().distttt = 17  

function AutoSwat()
    VIM:SendKeyEvent(true, Enum.KeyCode.R, false, game)
    wait(0.1)
    VIM:SendKeyEvent(false, Enum.KeyCode.R, false, game)
end

game:GetService("RunService").RenderStepped:Connect(function()
    for index, ball in pairs(workspace:GetChildren()) do
        if ball.Name == "Football" and ball:IsA("BasePart") then
            if charrr:FindFirstChild("HumanoidRootPart") and (charrr:FindFirstChild("HumanoidRootPart").Position - ball.Position).Magnitude <= distttt and autoSwatOn then
                AutoSwat()
            end
        end
    end
end)
		
	
t5:Toggle("Auto Catch", {
	Default  = false,
	Callback = function(v)
		autoCatchOn = v
	end,
})
t5:Slider("Auto Catch Distance", {
	Default  = 10,
	Min		 = 10,
	Max		 = 20,
	Callback = function(v)
		_G.dist = v
	end,
})
t5:Toggle("Auto Jump", {
	Default  = false,
	Callback = function(v)
		autoJumpOn = v
	end,
})
t5:Slider("Auto Jump Distance", {
	Default  = 10,
	Min		 = 10,
	Max		 = 20,
	Callback = function(v)
		disttt = v
	end,
})
t5:Toggle("Auto Swat", {
	Default  = false,
	Callback = function(v)
		autoSwatOn = v
	end,
})
t5:Slider("Auto Swat Distance", {
	Default  = 10,
	Min		 = 10,
	Max		 = 20,
	Callback = function(v)
		distttt = v
	end,
})
t5:Toggle("Auto Rush", {
	Default  = false,
	Callback = function(v)
		aRushOn = v
	end,
})
t5:Slider("Auto Rush Distance", {
	Default  = 20,
	Min		 = 0,
	Max		 = 30,
	Callback = function(v)
		agDist = v
	end,
})
t5:Toggle("Auto Captain", {
	Default  = false,
	Callback = function(v)
		autoCapOn = v
	end,
})


local jps = 50
        local jumpPowerEnabled = false
        local connection
        local lp = Players.LocalPlayer
        local char = plr.Character
        lp.CharacterAdded:Connect(function(character)
            char = character
        end)
        local function onToggle(Value)
            jumpPowerEnabled = Value
            if jumpPowerEnabled then
                connection = game:GetService("RunService").RenderStepped:Connect(function()
                    local upWard = Vector3.new(0, 10, 0)
                    local lp = Players.LocalPlayer
                    local hrp = char and char:FindFirstChild("HumanoidRootPart")
                    local hum = char and char:FindFirstChild("Humanoid")
                    local newJPS = (jps / 10)
                    if hrp and hum and hum.FloorMaterial == Enum.Material.Grass and hum.Jump then
                        hrp.Velocity = upWard * newJPS
                    end
                end)
            else
                if connection then
                    connection:Disconnect()
                    connection = nil
                end
            end
        end



        
getgenv().LookVectorSpeed = false
getgenv().UIS = game:GetService("UserInputService")
getgenv().playAH = game:GetService("Players").LocalPlayer
getgenv().ggchara = playAH.Character or playAH.CharacterAdded:Wait()
getgenv().hrpp = nil
getgenv().Speedboostnum = 3
playAH.CharacterAdded:Connect(function(gg)
    ggchara = gg
end)
task.spawn(function()
    while wait() do
        hrpp = ggchara:FindFirstChild("HumanoidRootPart")
    end
end)

function VectorSpeed()
    hrpp = ggchara:FindFirstChild("HumanoidRootPart")

    if LookVectorSpeed  then
        hrpp.CFrame = hrpp.CFrame + hrpp.CFrame.LookVector * Speedboostnum
    end
end

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F then
        VectorSpeed()
    end
end)

		t2:Toggle("Jump Power", {
			Default  = false,
			Callback = function(v)
				local jpOnDude = v
				onToggle(jpOnDude)
			end,
		})
		t2:Slider("Jump Power Strength", {
			Default  = 50,
			Min		 = 50,
			Max		 = 70,
			Callback = function(v)
				jps = v
			end,
		})


		t2:Toggle("Angle Enhancer (Dont Use with JP)", {
			Default  = false,
			Callback = function(v)
				local jpOnDudee = v
				onToggle2(jpOnDudee)
			end,
		})
		t2:Toggle("Anti Jam", {
			Default  = false,
			Callback = function(v)
				antiJamOn = v
			end,
		})
		t2:Toggle("Anti Block", {
			Default  = false,
			Callback = function(v)
				antiBlockOn = v
			end,
		})
		t2:Toggle("Speed Boost | F", {
			Default  = false,
			Callback = function(v)
				LookVectorSpeed = v
			end,
		})
		t2:Slider("Speed Boost Strength", {
			Default  = 3,
			Min		 = 0,
			Max		 = 5,
			Callback = function(v)
				Speedboostnum = v
			end,
		})
		t3:Toggle("QB Aimbot", {
			Default  = false,
			Callback = function(v)
				state = v
			end,
		})
		t3:Toggle("Auto Angle", {
			Default  = false,
			Callback = function(v)
				AutoAngie = v
			end,
		})
		t4:Toggle("Full Beam Length", {
			Default  = false,
			Callback = function(v)
				customBeam = v
			end,
		})
		t4:Toggle("Remove Beam End Part", {
			Default  = false,
			Callback = function(v)
				endPartOfBeam = v
			end,
		})
		t4:Toggle("Custom Lead", {
			Default  = false,
			Callback = function(v)
				customLeads = v
			end,
		})
		t4:Slider("Lead Distance", {
			Default  = 18.9,
			Min		 = 15,
			Max		 = 20,
			Callback = function(v)
				customLead = v
			end,
		})
		t3:Toggle("Auto Power", {
			Default  = false,
			Callback = function(v)
				AutoPowa = v
			end,
		})
		t3:Toggle("High Power Mode", {
			Default  = false,
			Callback = function(v)
				Highestpwrmode = v
			end,
		})
		t3:Toggle("Auto Mode Selection", {
			Default  = false,
			Callback = function(v)
				autopmode = v
			end,
		})
		t3:Toggle("Auto Select WR", {
			Default  = false,
			Callback = function(v)
				autoswr = v
			end,
		})
		t3:Toggle("R and F to change angle manually", {
			Default  = false,
			Callback = function(v)
				print("ok")
			end,
		})
		t3:Toggle("Z and X to change power manually", {
			Default  = false,
			Callback = function(v)
				print("ok")
			end,
		})
		t3:Toggle("C to change modes", {
			Default  = false,
			Callback = function(v)
				print("ok")
			end,
		})
		t6:Toggle("Magnet Hitbox Visualizer", {
			Default  = false,
			Callback = function(v)
				on3 = v
			end,
		})
		t6:Toggle("Ball Trajectory Beam", {
			Default  = false,
			Callback = function(v)
				BeOn = v
			end,
		})
		t3:Toggle("Q to lock on", {
			Default  = false,
			Callback = function(v)
				print("ok")
			end,
		})
