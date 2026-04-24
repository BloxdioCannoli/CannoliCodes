## World Code
> Paste this into World Code:

```ts
// Credit to Bloxdio Cannoli on YT

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

let textNPCs = [
    {
        pos: [-123, 15, 817],

        title: "The Mines",
        titlecolor: "gray",

        subtitle: "Welcome to the mines!",

        animation: "wave",

        textures: {
            head: "zombie"
        }
    },
    {
        pos: [-78, 14, 699],

        title: "The Farm",
        titlecolor: "lime",

        subtitle: "Let's start planting.",

        animation: "crouch",

        textures: {
            head: "farmer"
        }
    },
    {
        pos: [-150, 16, 695],

        title: "The Nether",
        titlecolor: "purple",

        subtitle: "Follow me...",

        nospin: true,

        textures: {
            head: "portal_mage"
        }
    },
];

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
    }

    if (doSpawnText == "spawn") {
        let text = textNPCs[textNum];

        let [x, y, z] = text.pos;

        let autorotate=!text.nospin
        let npc = api.attemptCreateMeshEntity("Person", {
            textures: text.textures,
            autoRotate: autorotate,

            hideDist: 1000000,

        }, "");
        api.setPosition(npc, [x + 0.5, y + 1, z + 0.5]);

        textNPCs[textNum].npc = npc;


        api.setTargetedPlayerSettingForEveryone(npc, "nameTagInfo", {
            content: [
                {
                    str: text.title, style: {
                        color: text.titlecolor,
                        fontSize: "100px",
                    }
                }
            ], backgroundColor: "rgba(0,0,0,0)",

            subtitle: [
                {
                    str: text.subtitle
                }
            ],
        });

        if (text.animation) {
            eval(`${text.animation}("${text.npc}")`);
        }

        textNum++;

        if (textNum >= textNPCs.length) {
            doSpawnText = "ready2";
            tickDelay = 3;
            textNum = 0;
        }
    } else if (doSpawnText == "animate") {
        let text = textNPCs[textNum];

        if (text.animation) {
            eval(`${text.animation}("${text.npc}")`);
            //wave(text.npc);
        }

        textNum++;

        if (textNum >= textNPCs.length) {
            doSpawnText = false;
            textNum = 0;
        }
    }
}

function crouch(id) {
    api.animateEntity(id, {
        animationDurationMs: 1000,
        loop: true,
        nodeAnimations: {
            TorsoNode: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                    { timeFraction: 0.4, rotation: { point: [0.5, 0, 0] } },
                    { timeFraction: 0.8, rotation: { point: [0, 0, 0] } },
                ]
            },

            HeadMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                    { timeFraction: 0.4, rotation: { point: [-0.5, 0, 0] } },
                    { timeFraction: 0.8, rotation: { point: [0, 0, 0] } },
                ]
            },

            ArmRightMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            ArmLeftMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            LegRightMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            LegLeftMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            }
        }
    });
};

function wave(id) {
    api.animateEntity(id, {
        animationDurationMs: 1500,
        loop: true,
        nodeAnimations: {
            TorsoNode: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            HeadMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            ArmRightMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                    { timeFraction: 0.3, rotation: { point: [-2.5, -1, 0] } },
                    { timeFraction: 0.4, rotation: { point: [-2.5, -0.5, 0.1] } },
                    { timeFraction: 0.6, rotation: { point: [-2.5, -0.5, -0.2] } },

                    { timeFraction: 0.7, rotation: { point: [-2.5, -0.5, 0] } },
                    { timeFraction: 0.8, rotation: { point: [-2.5, 0, 0] } },

                    { timeFraction: 0.9, rotation: { point: [0, 0, 0] } },
                    { timeFraction: 1, rotation: { point: [0, 0, 0] } },
                ]
            },

            ArmLeftMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            LegRightMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            },

            LegLeftMesh: {
                timeline: [
                    { timeFraction: 0.0, rotation: { point: [0, 0, 0] } },
                ]
            }
        }
    });
}
```
