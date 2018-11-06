# CS 220 - Synthesis of Digital Systems

This device is generally based on the [ParchMint specification](), however there are a few additional pieces of information that you may need in order to perform fine-grained routing. 

## Params

Add additional key `params` (parameters) has been added to each component object under the `components` key. Within this there are two additional arrays `valves` and `groups` which represent the internal structure of that component.

### Valves

The `valves` array contains a number of objects, each of which represents a single valve within that component.

```
{
    "id": "unique-valve-id-001",
    "internal-connections": [
        "unique-valve-id-002",
        "unique-valve-id-003"
    ]
}
```

Each object contains an `id` field which is an id that is unique to the component (but not necessarily unique across components) as well as an `internal-connections` array. The `internal-connections` array contains a list of other valve ids which this valve can be connected to internally to the component. If the `internal-connections` array is empty, then that valve does not connect to any other valves within the component.

### Groups

The `groups` array contains some valve groups, where a group is a collection of valves listed in the `valves` array and not all those valves need to be connected to an external control line (making some number of them optional).

```
{
    "id": "unique-group-id",
    "valves": [
        "unique-valve-id-001",
        "unique-valve-id-002",
        "unique-valve-id-003"
    ]
    "num-required": 2
}
```

Each object contains an `id` field which is an id that is unique to the component (but not necessarily across compoents) as well as a `valves` array, which lists all the valves that are contained within that group. The last field, `num-required` specififes how many of the valves within that group must be used. Any valves that are not listed in a group **must** be connected to a control line.

## Ports

In addition to the totally new key introduced into the component objects, there is also a new key within the ports array of the component object. Some port objects now contain a `valve` key, which represents that the port at that location is connected to that particular valve from the valve array mentioned above. This information is necessary for the fine grained routing step for two reasons: (1) Some valves can be connected to from a number of different ports and (2) you can enter a component through a port connected to a valve, and use one of the internal connections (mentioned above) to leave the component from another location.
