# qb-oxyruns
No-Pixel Oxy runs converted to QB-Core framework. (latest one)

dependecies 
qb-core 
qb-smallresources 

Add this to you qb-smallresources/client/consumables.lua

```lua
RegisterNetEvent("consumables:client:Oxy")
AddEventHandler("consumables:client:Oxy", function(itemName)
    TriggerEvent('animations:client:EmoteCommandStart', {"drink"})
    QBCore.Functions.Progressbar("drink_something", "Down the pills..", 2500, false, true, {
        disableMovement = false,
        disableCarMovement = false,
		disableMouse = false,
		disableCombat = true,
    }, {}, {}, {}, function() -- Done
        TriggerEvent("inventory:client:ItemBox", QBCore.Shared.Items[itemName], "remove")
        TriggerEvent('animations:client:EmoteCommandStart', {"c"})
        OxyEffect()
    end)
end)

function OxyEffect()
    if not onOxy then
        local RelieveOdd = math.random(35, 45)
        onOxy = true
        local OxyTime = 60
        Citizen.CreateThread(function()
            while onOxy do 
                SetPlayerHealthRechargeMultiplier(PlayerId(), 1.8)
                Citizen.Wait(1000)
                oxyTime = oxyTime - 1
                if oxyTime == RelieveOdd then
                    TriggerServerEvent('qb-hud:Server:RelieveStress', math.random(14, 18))
                end
                if oxyTime <= 0 then
                    onOxy = false
                end
            end
        end)
    end
end
```

add this to you qb-smallresources/server/consumables.lua

```lua
QBCore.Functions.CreateUseableItem("oxy", function(source, item)
    local Player = QBCore.Functions.GetPlayer(source)
	if Player.Functions.RemoveItem(item.name, 1, item.slot) then
        TriggerClientEvent("consumables:client:Oxy", source)
    end
end)
```


