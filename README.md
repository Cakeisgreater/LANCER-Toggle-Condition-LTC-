# LANCER-Toggle-Condition-LTC-
A module for Foundry. Contains macros for LANCER.
![toggle_demo](https://user-images.githubusercontent.com/129597129/233853428-5bd7fb8c-50a9-4d50-b92a-bc5aaec5eaac.gif)

## A macro compendium containing two things: 
1) Toggles for *most* of LANCER CRB's conditions and statuses (i.e. prone, stunned, invisible, etc.)
2) A series of Worldscripts. These set defaults for certain settings. Currently contains:
  - Token image resizer (~~"Resizer_new"~~ "Resizer v3")
  - Default Hex grid and token vision off for new scenes ("Scene Worldscript")
  - New conditions ("homebrew conditions") to indicate some NPC abilities. Also contains some homemade condition icons! You can easily add your own conditions. 

[Worldscripter](https://foundryvtt.com/packages/world-scripter) module recommended for installing worldscripts. 


## Installation (if you can call it that)
Contains two compendiums - "LTC - Condition Macros" and "LTC World Scripts". The former should be imported, and the latter should be added using World Scripter or setup as a world script (check foundry wiki for more!).


## recommended companion modules to improve your experience
- [Lancer Conditionss](https://github.com/Eranziel/lancer-conditions)
- [Quick Status select](https://foundryvtt.com/packages/quick-status-select)
- [Monk's little details](https://foundryvtt.com/packages/monks-little-details)
- [Dfreds Efects Panel](https://github.com/DFreds/dfreds-effects-panel)
- [Status Icon Counters](https://foundryvtt.com/packages/statuscounter)

## Things to keep in mind
- ~~The condition toggles can only be used by the GM~~. Should work for both player and GM.Thanks to Zhell on the Foundry Discord. **NOTE**: Cannot be used from player-side to toggle "Lockon" (or any other condition for that matter) on actors they DO NOT own (i.e. NPCs, etc.). **This is a (mostly) GM-side tool**.
  - Also this doesn't require Combat Utility Belt (CUB)! In my personal experience, CUB has tendency to bug out in LANCER 1.5.3 and presents certain compatibility issues. 
- The Worldscripts can be edited to your own preference. They look terrible and amateurish because I am not a programmer. The code was frankensteined together with help from Valk, Eranziel, CSMcFarland, and others on the PILOT NET discord.
- you can always add your own conditions by adding blocks to the "homebrew conditions" world script
  - furthermore you can easily make your own toggle macros by copying the syntax. 


## Thanks to PILOT NET and Foundry Discord for help with everything.

## Scripts Included
### Homebrew Conditions
```console.log("Adding custom status effects")
let conds = [
{
    id: "dispersal-shield",
    label: "Dispersal Shield",
    icon: "Icons/Dispersal-Shield.png",
},
{
    id: "eye-of-midnight", 
	label: "Eye of Midnight", 
	icon: "Icons/Eye-of-midnight.png",
},
{
    id: "bodyguard", 
	label: "Bodyguard", 
	icon: "Icons/Bodyguard.png",
},
{
	id: "leadership", 
	label: "Leadership Die", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/dice-d6-white.png",
},
{
	id: "lh-javelins", 
	label: "Lock/Hold Javelins", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/LockHold-Javelins.png",
},
{
	id: "retribution", 
	label: "Retribution", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/Retribution.png",
},
{
	id: "explosive-knives", 
	label: "Explosive Knives", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/Explosive-Knives.png",
},
{
	id: "pin", 
	label: "Pin", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/pin.png",
},
{
	id: "coreworm-rockets", 
	label: "Coreworm Rockets", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/core-worm-rockets.png",
},
{
	id: "erupting-shrapnel", 
	label: "Erupting Shrapnel", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/erupting shrapnel.png",
},
{
	id: "tracker", 
	label: "Tracker", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/Tracker.png",
},
{
	id: "orange", 
	label: "Orange", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/orange.png",
},
{
	id: "apple", 
	label: "Apple", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/apple.png",
},
{
	id: "blueberry", 
	label: "Blueberry", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/blueberry.png",
},
{
	id: "lemon", 
	label: "Lemon", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/lemon.png",
},
{
	id: "lime", 
	label: "Lime", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/lime.png",
},
{
	id: "plum", 
	label: "Plum", 
	icon: "modules/lancer-toggle-conditions-ltc/Icons/plum.png",
}
];
CONFIG.statusEffects = CONFIG.statusEffects.concat(conds);
```
### Sample Toggle Condition
```const id = "your-condition-here";
const eff = CONFIG.statusEffects.find(e => e.id === id);
await token.toggleEffect(eff);
```
### Resizer (v3)
```
console.log("Creating Token Augment Hook");
Hooks.on("preCreateToken", (token, data, options, userId) => {

	let Token_Size = token.actor.system.derived.mm.Size;
	let texture = foundry.utils.duplicate(token.texture);
	let flags = foundry.utils.duplicate(token.flags);
	if (Token_Size <= 1 && !token.actor.flags["lancer-clocks"] && token.actor.type !== 'deployable') {
        console.log(`Scaling up Size 1 token and offsetting for ${token.name}`);
        texture.scaleX = 1.5;
        texture.scaleY = 1.5;
        flags["hex-size-support"].pivotx = 0;
        flags["hex-size-support"].pivoty = 50;
        token.updateSource({"texture": texture, "flags": flags});
}
	else if (Token_Size === 2 && !token.actor.flags["lancer-clocks"] && token.actor.type !== 'deployable') {
	console.log(`Scaling up Size 2`);
        texture.scaleX = 1.3;
        texture.scaleY = 1.3;
        flags["hex-size-support"].pivotx = 0;
        flags["hex-size-support"].pivoty = 40;
        token.updateSource({"texture": texture, "flags": flags});
	}
	else if (Token_Size === 3 && !token.actor.flags["lancer-clocks"] && token.actor.type !== 'deployable') {
	console.log(`Scaling up Size 3`);
        texture.scaleX = 1.0;
        texture.scaleY = 1.0;
        flags["hex-size-support"].pivotx = 0;
        flags["hex-size-support"].pivoty = 40;
        token.updateSource({"texture": texture, "flags": flags});
	}
	});
  ```
### Scene Worldscript
```
console.log("Setting hex grids and turning off token vision...");
Hooks.on("preCreateScene", (scene) => {
	scene.updateSource({tokenVision: false, "grid.type": CONST.GRID_TYPES.HEXODDR});
});
```
# manifest:
https://github.com/Cakeisgreater/LANCER-Toggle-Condition-LTC-/releases/download/Latest/module.json

### check out my other stuff!
https://github.com/Cakeisgreater/Lancer-Token-Wildcard
