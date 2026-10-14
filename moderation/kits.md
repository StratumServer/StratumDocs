---
title: "Kits"
description: "Staff-made item kits: create them in game, assign them, and let players redeem them."
category: "Moderation"
categoryOrder: 5
order: 3
---

Kits are item bundles that staff build in game. Players redeem them with `/kit`, and staff manage them with `/kitedit`. You never edit a kit file by hand. Both commands work with the normal game client, so there is no separate interface to install.

## Access

`/kit` needs the `stratum.kits` privilege, and `/kitedit` needs `stratum.kitedit`. Roles with auto-grant, normally `admin`, receive both. Give them to other roles explicitly with `/roles grant <rolename> stratum.kits`. Check a role's privileges with `/roles info <rolename>`.

You can switch either command off with `Commands.Kits.Enabled` or `Commands.KitEdit.Enabled` in `stratum-commands.json`. A change takes effect after a restart.

## Players: /kit

```
/kit
/kit <name>
```

`/kit` with no name lists the kits you can redeem. Each entry shows its item count, its cooldown if it has one, and whether it is once per life. `/kit <name>` redeems the kit now.

A kit with no role assignment is open to every player with kit access. A kit assigned to roles is open only to players who hold one of those roles.

Each kit can have a cooldown. Cooldowns are tracked per player and kit, and they reset when the server restarts. Staff with the staff chat privilege skip the kit cooldown.

A kit marked once per life can be redeemed once between deaths.

A kit marked to give on respawn is handed out automatically after the player respawns. It counts as redeemed for that life.

Armor goes straight into a matching empty slot. Anything else goes to the player's inventory. If the inventory is full, the item is dropped at the player's feet. The message after a redeem says how many items were given, dropped, or failed.

## Staff: /kitedit

```
/kitedit list
/kitedit create <name>
/kitedit additem <name>
/kitedit removeitem <name> <index>
/kitedit preview <name>
/kitedit delete <name>
/kitedit rename <name> <newName>
/kitedit setrole <name> <role>
/kitedit setcooldown <name> <seconds>
/kitedit setscope <name> all|hotbar
/kitedit onrespawn <name> on|off
/kitedit oneperlife <name> on|off
/kitedit give <name> <player>
```

`create` takes a snapshot of your inventory and makes it the kit's item list. If the kit exists, its list is replaced.

`additem` adds the item in your active hotbar slot.

`preview` shows the kit's assignment, cooldown, flags, and numbered items. Use the numbers with `removeitem`.

`setscope` controls what a later `create` captures:

- `all` (the default) captures your hotbar and your worn items, including armor and a worn backpack.
- `hotbar` captures only the hotbar, including the offhand slot. Use it when the kit should hand out tools or consumables and never gear.

Changing the scope does not change an existing kit. To narrow a kit, set the scope and run `create` again.

`give` gives the kit to an online player right away.

Action words are lowercase. Kit names are not case sensitive.
