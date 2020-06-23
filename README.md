# ItsZenEXE Overwatch Gamemode

This is ItsZen_EXE's OW Gamemode code

To use it in-game, start a custom game, press settings, copy the contents of the file [OWStrife.txt](OWStrife.txt) and select import settings

If there is a PTR update on Overwatch, this will most likely be updated to use that update!

You can find a Patch Notes for this custom game at [PatchNotes.md](PatchNotes.md)

My discord server should also get updates automatically on my progress!

variables
{
	player:
		40: fastcooldowns
		41: CommandWaitTime
}

rule("Enable/Disable using ult charge saying")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Is Communicating(Event Player, Ultimate Status) == True;
	}

	actions
	{
		If(Is In Spawn Room(Event Player));
			Small Message(Event Player, Custom String("Unable to enable short cooldowns (In Spawn)"));
		Else;
			If(Event Player.fastcooldowns == False);
				Small Message(Event Player, Custom String("Short-Cooldowns Enabled (wait 4s to disable)"));
				Event Player.fastcooldowns = True;
			Else;
				Event Player.fastcooldowns = False;
				Small Message(Event Player, Custom String("Short-Cooldowns Disabled (wait 4s to enable)"));
	}
}

rule("secondary fire")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Ability Cooldown(Event Player, Button(Secondary Fire)) > 1.500;
		Event Player.fastcooldowns == True;
	}

	actions
	{
		Set Ability Cooldown(Event Player, Button(Secondary Fire), 1);
	}
}

rule("ability 1")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Ability Cooldown(Event Player, Button(Ability 1)) > 1.500;
		Event Player.fastcooldowns == True;
	}

	actions
	{
		Set Ability Cooldown(Event Player, Button(Ability 1), 1);
	}
}

rule("ability 2")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Ability Cooldown(Event Player, Button(Ability 2)) > 1.500;
		Event Player.fastcooldowns == True;
	}

	actions
	{
		Set Ability Cooldown(Event Player, Button(Ability 2), 1);
	}
}

rule("ult")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Ultimate Charge Percent(Event Player) != 100;
		Event Player.fastcooldowns == True;
	}

	actions
	{
		Abort If Condition Is False;
		While(Is Using Ultimate(Event Player) == True);
			Wait(0.250, Ignore Condition);
		Else;
		End;
		Wait(1, Ignore Condition);
		Abort If Condition Is False;
		Set Ultimate Charge(Event Player, 100);
	}
}

rule("crouch")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Ability Cooldown(Event Player, Button(Crouch)) > 1.500;
		Event Player.fastcooldowns == True;
	}

	actions
	{
		Set Ability Cooldown(Event Player, Button(Crouch), 1);
	}
}

rule("player is in spawn room")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Is In Spawn Room(Event Player) == True;
	}

	actions
	{
		If(Event Player.fastcooldowns == True);
			Small Message(Event Player, Custom String("Short cooldown disabled (in spawn)"));
			Event Player.fastcooldowns = False;
		Else;
		End;
	}
}

rule("cooldown timer")
{
	event
	{
		Ongoing - Each Player;
		All;
		All;
	}

	conditions
	{
		Is Communicating(Event Player, Ultimate Status) == True;
	}

	actions
	{
		Create HUD Text(Event Player, Custom String("Command Wait Time: {0}", Event Player.CommandWaitTime), Null, Null, Left, -5, Red,
			White, White, Visible To and String, Default Visibility);
		Event Player.CommandWaitTime = 4;
		Wait(1, Ignore Condition);
		Event Player.CommandWaitTime = 3;
		Wait(1, Ignore Condition);
		Event Player.CommandWaitTime = 2;
		Wait(1, Ignore Condition);
		Event Player.CommandWaitTime = 1;
		Wait(1, Ignore Condition);
		Destroy HUD Text(Last Text ID);
	}
}
