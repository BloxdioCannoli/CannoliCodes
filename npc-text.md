```ts
/*
Credits to Bloxdio Cannoli on YT
*/

/*
Available preset head textures:

chef
farmer
farmer_gill
monster_hunter_lorenzo
painter_spencer
piggy_banker
portal_mage
trader
trader_black
trader_blue
wizard
zombie
*/

// modify the contents of this array by adding an object:
/*
template:
{
  pos: [x, y, z],

  title: "TITLE",
  titlecolor: "COLOR",

  subtitle: "SUBTITLE",

  animation: "wave or crouch",

  textures: {
    head: "preset skin"
  }
}
*/
let textNPCs = [

];


function removeAllMesh() {
    let ents = api.getEntitiesInRect([-10000, -10000, -10000], [10000, 10000, 10000]);
    for (let e of ents) {
        if (api.getEntityType(e) == "Mesh") {
            api.deleteMeshEntity(e);
        }
    }
}; removeAllMesh();

function spawnNPC(text, color, x, y, z) {
    entity = api.attemptCreateMeshEntity("BloxdBlock",
        {
            blockName: "Invisible Solid",
            size: 1,
        });
    api.setPosition(entity, [x + 0.5, y + 2, z + 0.5]);

    api.setTargetedPlayerSettingForEveryone(
        entity,
        "nameTagInfo",
        {
            content: [
                {
                    str: text,
                    style: {
                        color: color,
                        fontSize: "150px"
                    }
                },
            ], backgroundColor: ""
        }, true,
    );
}

let consec = 0; let wait = 0; let tickDelay = 5; let textNum = 0; let doSpawnText = "ready";
function tick() {
    if (wait > 0) { wait--; return; }; if (consec >= 2) { consec = 0; wait = 10; } else { consec++; }

    if (tickDelay > 0) {
        tickDelay--;
    } else {
        if (doSpawnText == "ready") {
            doSpawnText = "spawn";
        } else if (doSpawnText == "ready2") {
            doSpawnText = "animate";
}
```
