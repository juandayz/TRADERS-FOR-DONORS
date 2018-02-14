# TRADERS-FOR-DONORS

1.Open your custom fn_selfactions.sqf

Find this block:
```
// All Traders
	if (_isMan && {!(isPlayer _cursorTarget)} && {_typeOfCursorTarget in serverTraders} && {!_isPZombie}) then {
		if (s_player_parts_crtl < 0) then {
			_humanity = player getVariable ["humanity",0];
			_traderMenu = call compile format["menu_%1;",_typeOfCursorTarget];		
			_low_high = localize "STR_EPOCH_ACTIONS_HUMANITY_LOW";
			_humanity_logic = false;
			if ((_traderMenu select 2) == "friendly") then {
				_humanity_logic = (_humanity < -5000);
			};
			if ((_traderMenu select 2) == "hostile") then {
				_low_high = localize "STR_EPOCH_ACTIONS_HUMANITY_HIGH";
				_humanity_logic = (_humanity > -5000);
			};
			if ((_traderMenu select 2) == "hero") then {
				_humanity_logic = (_humanity < 5000);
			};
			if (_humanity_logic) then {
				_cancel = player addAction [format[localize "STR_EPOCH_ACTIONS_HUMANITY",_low_high], "\z\addons\dayz_code\actions\trade_cancel.sqf",["na"], 0, true, false];
				s_player_parts set [count s_player_parts,_cancel];
			} else {
				// Static Menu
				{
					_buy = player addAction [format["Trade %1 %2 for %3 %4",(_x select 3),(_x select 5),(_x select 2),(_x select 6)], "\z\addons\dayz_code\actions\trade_items_wo_db.sqf",[(_x select 0),(_x select 1),(_x select 2),(_x select 3),(_x select 4),(_x select 5),(_x select 6)], (_x select 7), true, true];
					s_player_parts set [count s_player_parts,_buy];		
				} count (_traderMenu select 1);
				if (DZE_ConfigTrader) then {
					_buyV = player addAction [localize "STR_EPOCH_PLAYER_289", "\z\addons\dayz_code\actions\AdvancedTrading\init.sqf",(_traderMenu select 0), 999, true, false];
					s_player_parts set [count s_player_parts,_buyV];
				} else {
					// Database menu
					_buy = player addAction [localize "STR_EPOCH_PLAYER_289", "\z\addons\dayz_code\actions\show_dialog.sqf",(_traderMenu select 0), 999, true, false];
					s_player_parts set [count s_player_parts,_buy];
				};
			};
			s_player_parts_crtl = 1;	
		};
	} else {
		{player removeAction _x} count s_player_parts;s_player_parts = [];
		s_player_parts_crtl = -1;
	};
  ```
 
 Replace the whole block by:
 
 ```ruby
 // All Traders
    if (_isMan && {!(isPlayer _cursorTarget)} && {_typeOfCursorTarget in serverTraders} && {!_isPZombie}) then {
            if (s_player_parts_crtl < 0) then {
            _humanity = player getVariable ["humanity",0];
            _traderMenu = call compile format["menu_%1;",_typeOfCursorTarget];        
            _low_high = "";
            _humanity_logic = false;
            
             
            if((_traderMenu select 2) == "donor") then {
               _humanity_logic = (player getVariable ["donor",0] == 0);
               _low_high="your not a donor";
            }; 
            
            
            if ((_traderMenu select 2) == "friendly") then {
                _humanity_logic = (_humanity < -5000);
                _low_high="more than -10000 of Humanity";
            };
            if ((_traderMenu select 2) == "hostile") then {
               
                _humanity_logic = (_humanity > -5000);
                _low_high="humanity to high";
            };
            if ((_traderMenu select 2) == "hero") then {
                _humanity_logic = (_humanity < 5000);
                _low_high="humanity to low";
            };
            if (_humanity_logic) then {
                _cancel = player addAction  [format["You cannot access: %1 !",_low_high], "\z\addons\dayz_code\actions\trade_cancel.sqf",["na"], 0, true, false];
                s_player_parts set [count s_player_parts,_cancel];
            } else {
                // Static Menu
                {
                    _buy = player addAction [format["Trade %1 %2 for %3 %4",(_x select 3),(_x select 5),(_x select 2),(_x select 6)], "\z\addons\dayz_code\actions\trade_items_wo_db.sqf",[(_x select 0),(_x select 1),(_x select 2),(_x select 3),(_x select 4),(_x select 5),(_x select 6)], (_x select 7), true, true];
                    s_player_parts set [count s_player_parts,_buy];        
                } count (_traderMenu select 1);
                if (DZE_ConfigTrader) then {
                    _buyV = player addAction [localize "STR_EPOCH_PLAYER_289", "\z\addons\dayz_code\actions\AdvancedTrading\init.sqf",(_traderMenu select 0), 999, true, false];
                    s_player_parts set [count s_player_parts,_buyV];
                } else {
                    // Database menu
                    _buy = player addAction [localize "STR_EPOCH_PLAYER_289", "\z\addons\dayz_code\actions\show_dialog.sqf",(_traderMenu select 0), 999, true, false];
                    s_player_parts set [count s_player_parts,_buy];
                };
            };
            s_player_parts_crtl = 1;    
        };
    } else {
        {player removeAction _x} count s_player_parts;s_player_parts = [];
        s_player_parts_crtl = -1;
    };  
 ```
  
  2.Open you init.sqf and at very bottom paste:  (very bottom means very bottom, not before the last }; ok?)
  
```ruby
donorlist = [""];//enter your donors ids
if ((getPlayerUID player) in donorlist) then {player setvariable ["donor",1,true];}else{player setvariable ["donor",0,true];};
```

3.Now you need enter your new AI for donor trader... I dont know what map youre using... but for example if u use chernarus11.

Open ...@DayZ_Epoch_Server_taki\addons\dayz_server\traders\chernarus11.sqf and add your new trader class name and coords here:
```ruby
[
  ["YOUR TRADER CLASS NAME",[COORDS],SET DIR],//ENTER YOUR DONOR TRADER HERE
	["Profiteer4",[11449.5,11341,0],34.5259],
	["RU_Villager3",[7996.1,2899.08,0.669153],86.8589],
	["Worker3",[4041.62,11668.9,0],24.9128],
	["CIV_EuroMan01_EP1",[4064.07,11680.1,0],231.007],
	["RU_WorkWoman5",[4071.99,11676.7,0],206.817],
  ............more and more ai.............
] call server_spawnTraders;
```

4.Open your server_traders.sqf and at very top add your donor trader class name:
```ruby serverTraders = ["YOur TRADER CLASS NAME","Doctor","RU_Hooker3","Haris_Press_EP1","Woodlander4"....more traders];```

At very bottom add:
```ruby
menu_yourTraderClassName= [[["WhateverName",category number],["WhateverName",category number]],[],"donor"];//traderdonor
```
example:
```
menu_RU_Profiteer2= [[["Weapons",1050],["Ammo",1051]],[],"donor"];//traderdonor
```
