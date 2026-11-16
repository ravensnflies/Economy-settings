# PhunEconomy — Custom PhunMart Economy Configuration

**Collector Shops, Vehicle Dealerships, Pawn Services & Server Economy Integration for Project Zomboid Build 42**

Developed and configured by **Raven**

## Overview

PhunEconomy is a collection of JSON configuration files designed to extend UburGeek's PhunMart system into a more comprehensive multiplayer server economy.

The project establishes specialized shops, collectible-item exchanges, premium vehicle dealerships, medical and equipment vendors, and multiple forms of currency.

Rather than replacing PhunMart's underlying framework, PhunEconomy uses its configurable shops, item groups, pricing definitions, offer pools, and special transaction actions.

The objective is to create a functioning server economy in which scavenging, collecting, vehicle ownership, and trading provide meaningful progression opportunities.

**This repository contains configuration data, not a standalone mod or a replacement for PhunMart.**

## Features

### Multi-Vendor Shop System

The configuration defines six shop entries:

| Shop | Purpose |
|---|---|
| **Prawn Stars** | Pawn-shop item exchange and currency conversion |
| **Pitty The Tool** | Equipment and tools, including a dedicated generator offer |
| **CSV Pharmacy** | Medical supplies, consumables, and workout supplements |
| **Final Amendment** | Guns of Marz ammunition distribution |
| **Wrent-A-Wreck** | Premium civilian and military vehicle dealership |
| **Collectors** | Collectible-item exchanges using token rewards |

Each vendor is configured through PhunMart's pool and item systems.

### Collector Economy

The collector system creates an alternative use for scavenged objects that would otherwise have little practical value.

The supplied configuration contains a dedicated collection of 140 item identifiers, including:

- Vanilla and modded plush toys
- Spiffo collectibles
- Toy and novelty items
- Fossils, skulls, and biological specimens
- Crystals and mineral specimens
- Antique and unusual objects
- Trophies and medals
- Other decorative collectibles

Collector items reference the `token_collector_t1` reward definition.

The intention is to make exploration and collecting economically rewarding while creating demand for uncommon loot.

**Note:** `token_collector_t1` is referenced but not defined in the supplied custom Specials file. Its availability and payout depend on the installed PhunMart configuration.

### Vehicle Dealership — Wrent-A-Wreck

Wrent-A-Wreck provides a premium vehicle shop using token-based purchasing.

The civilian specialty vehicle group includes:

- Pursuit Special II
- DeLorean DMC-12
- Dodge Charger Demon
- Ford Elgin Special
- Chevrolet Camaro
- Lamborghini Countach
- Nissan GT-R
- Volkswagen Beetle
- Range Rover

The group uses a default price of **150 tokens**, with separate item-level pricing entries where specified.

#### Military Vehicle Imports

A second vehicle pool provides military-oriented vehicles from the Nyx vehicle collection.

| Vehicle family | Configured price |
|---|---:|
| Nyx Strider | 600 tokens |
| Nyx Ifrit | 900 tokens |
| Nyx Hunter | 1,100 tokens |

Multiple camouflage and color variants are included.

#### Vehicle Offer Configuration

The specialty and military vehicle groups define:

- Single-unit stock for configured offers
- 100% vehicle condition
- Fuel between 90% and 100%
- Separate civilian and military inventory pools
- Dedicated vehicle selection groups

Vehicle availability depends on the corresponding vehicle mods and PhunMart's support for the configured vehicle identifiers.

These files do not independently blacklist vehicles from ordinary world spawning.

### PhunKeys Vehicle Trade-In Integration

PhunEconomy includes configuration supporting the companion PhunKeys vehicle-transmutation system.

The configuration registers:

- `PhunKeys.TransmutationBox` as a purchasable item
- `PhunKeys.VehicleKeySeller` as a trade-in item
- A dedicated collector action named `phunkeys_tradein_12`

The uploaded configuration awards **12 bound tokens** for a qualifying Vehicle Seller Key.

This creates an intended economy loop:

1. Obtain an eligible vehicle.
2. Use PhunKeys to convert the vehicle into a trade-in key.
3. Exchange the key through the configured collector transaction.
4. Receive bound tokens.
5. Use accumulated tokens toward eligible premium purchases.

Vehicle conversion logic and ownership validation are implemented by PhunKeys; this repository configures the corresponding PhunMart transactions.

### Pawn Shop and Cash Economy

Prawn Stars provides a conventional item-exchange economy using PhunMart's cash balance.

Example configured values include:

| Item | Cash payout |
|---|---:|
| Money Bundle | $50 |
| Payday Money Pile | $500 |
| Diamond | $13 |
| Emerald | $12 |
| Amethyst | $10 |
| Leather Harness Panties | $1 |

The configuration also includes payout references for sapphire and ruby items.

The displayed titles and special definitions should be reviewed together when modifying payout values.

### Precious Metal Exchanges

Precious metals use a separate bound-token reward mechanism.

| Item | Token reward |
|---|---:|
| Gold Ingot | 72 tokens |
| Silver Ingot | 24 tokens |
| PhunKeys Vehicle Trade-In | 12 tokens |

These transactions use the configured `grantBoundTokens` action rather than an ordinary cash-balance adjustment.

This separation allows the server economy to distinguish ordinary purchasing power from premium collectible and vehicle rewards.

### Medical and Supplement Shop

CSV Pharmacy uses a persistent offer pool containing medical items, recovery supplies, combat drinks, and supplements.

The configured inventory includes:

- Suture needles and forceps
- Bandages and tweezers
- Painkillers and antibiotics
- Medical shears
- PhunCure
- Combat consumables
- CREATINE! supplements
- Junkiez More Workout Supplements products

This integrates the companion CREATINE! progression mod into the server's economy by making its supplement item available through a vendor.

### Tools and Generator Availability

Pitty The Tool uses a dedicated sticky generator offer while excluding generator variants from its ordinary pool.

The configuration sets:

**Generator price: $100**

The purpose is to prevent generator pricing and availability from being left entirely to an unrestricted random item pool.

### Ammunition Vendor

Final Amendment integrates ammunition from Guns of Marz.

Its sticky ammunition pool includes 26 configured ammunition item identifiers covering pistol, rifle, shotgun, and launcher ammunition.

Prices are assigned through shared pricing references rather than repeating monetary values in every shop entry.

## Technical Architecture

PhunEconomy is organized around six JSON files.

| File | Responsibility |
|---|---|
| `PhunMart_Shops.json` | Defines shop identities and assigned offer pools |
| `PhunMart_Pools.json` | Defines item sources, sticky inventories, and pool exclusions |
| `PhunMart_Groups.json` | Organizes items and vehicles into reusable groups |
| `PhunMart_Items.json` | Associates item identifiers with prices, rewards, and display titles |
| `PhunMart_Prices.json` | Defines reusable currency and token prices |
| `PhunMart_Specials.json` | Defines custom cash and token payout actions |

### Configuration-Driven Architecture

The system separates the shop definition from its available inventory and pricing logic.

For example:

`Shop → Pool → Item Group → Item Definition → Price or Special Action`

This separation allows individual parts of the economy to be modified without rewriting every shop.

A vehicle can be placed in a group, given a token price, and included in a dealership pool using reusable definitions.

Similarly, a collectible can reference a common reward action instead of requiring an independently defined payout mechanism for every item.

### Persistent Offers

Several shop pools are configured with `sticky: true`.

These include medical supplies, ammunition, generators, vehicle inventories, the vehicle-conversion box, and collectibles.

This is intended to provide more predictable product availability than purely randomized shop inventory.

Exact offer persistence and refresh behavior are controlled by the installed PhunMart implementation.

## Installation and Configuration

### Requirements

- A compatible Project Zomboid Build 42 environment
- PhunMart 2 and its required dependencies
- The corresponding third-party mods for any referenced custom items or vehicles

Examples of optional content integrations include:

- PhunKeys
- CREATINE!
- Junkiez More Workout Supplements
- Guns of Marz
- PhunCure
- MorePlushies
- Supported specialty and Nyx military vehicle mods

These third-party mods are needed only for the corresponding configured content, subject to PhunMart's handling of unavailable identifiers.

### Configuration Files

The archive contains:

```text
PhunMart_Prices.json
PhunMart_Items.json
PhunMart_Groups.json
PhunMart_Specials.json
PhunMart_Shops.json
PhunMart_Pools.json
```

These are PhunMart configuration overrides and must be placed or imported using the procedure appropriate to the installed PhunMart version.

**Back up your existing PhunMart configuration before replacing or merging files.**

Do not blindly overwrite an established server economy without reviewing its existing shop and item definitions.

### Customization

Server administrators can adapt the configuration by modifying:

- Shop inventory pools
- Item groups
- Cash and token prices
- Collector reward values
- Available vehicle models
- Premium vehicle prices
- Stock limits and offer weights
- Vendor availability and shop assignments

When adding new items, confirm that the item identifiers exist in the installed game and mod environment.

## Compatibility and Verification Notes

This configuration was developed for a particular modded server environment and is not presented as a universally tested drop-in package.

The supplied archive includes references that should be resolved against the active PhunMart installation:

- `pool_prawnstars_core` is referenced by the Prawn Stars shop but is not defined in the supplied custom Pools file.
- `token_collector_t1` is assigned to collectible items but is not defined in the supplied custom Specials file.
- `change_payout_payday_gold_bars` is referenced by an item but is not defined in the supplied custom Specials file.
- `token_24` is referenced by one vehicle definition but is not present in the supplied custom Prices file.
- The sapphire and ruby payout definitions contain removal markers instead of the same active `kind: pawn` structure used by several other gem payouts.

These references may depend on built-in definitions, prior server configuration, or intended overrides. Their existence or absence in the supplied archive alone does not establish whether a deployed PhunMart installation will resolve them.

Administrators should verify resulting offers, item availability, payout values, and shop behavior before using this configuration in a production economy.

## Development Methodology

**PhunEconomy was developed through an AI-assisted configuration, systems-integration, and iterative testing workflow directed by Raven.**

The project reflects a configuration-first approach to extending an existing software system.

My work focused on:

1. Researching PhunMart's exposed configuration framework.
2. Understanding how shops, pools, item groups, prices, and special actions interact.
3. Designing an economy with distinct cash and premium-token transactions.
4. Mapping third-party mod item identifiers into existing vendor systems.
5. Directing AI-assisted creation and revision of configuration data.
6. Investigating integration problems, inconsistent references, and unexpected behavior.
7. Refining item availability, payouts, and shop inventory to support the intended gameplay.

The project demonstrates practical familiarity with JSON configuration, data-driven software architecture, API and framework investigation, integration requirements, and software behavior evaluation.

I do not claim this project involved independently writing a new commerce engine or replacing the underlying PhunMart software.

The engineering contribution lies in adapting an existing framework into a coordinated multiplayer economy through configuration, integration research, and iterative development.

## Related Projects

- [PhunKeys](https://github.com/ravensnflies/PhunKeys) — Vehicle ownership, key transmutation, and vehicle trade-in functionality
- [CREATINE!](https://github.com/ravensnflies/creatine) — Fitness progression, supplements, and character abilities

## Credits

**Economy design, configuration architecture, integration research, and AI-assisted development:** Raven

**PhunMart / Phun ecosystem:** UburGeek and the respective contributors

**Third-party items and vehicles:** Their original mod authors

**Game:** Project Zomboid by The Indie Stone

---

*PhunEconomy is an independent configuration project. It is not affiliated with or endorsed by The Indie Stone or the developers of its third-party dependencies.*
