# Using HEH Data

All data held in the Holdem Heroes smart contract is freely available for developers to pull in and use in their own smart contracts and games. In addition to the standard NFT data and ownership, there are two datasets on which NFTs are built, and that are available for anyone to use:

1. Card Data
2. Hand Data

Data can be integrated by importing the `IHoldemHeroes.sol` interface contract, and pointing your contract to the deployed address.

## 1. Card Data

The card dataset holds information on a standard 52 card deck. It is held statically within the contract and is accessible by passing an integer ID from 0 - 51, to represent each card. The data is held such that queries for the same card ID are guaranteed to yield the same result with every request. For example, querying for card ID 24 will always return the data for Eight of Clubs.

Further, each card's component data is available for anyone who requires a more granular breakdown of each card.

Card data is available in multiple formats:

1. Component data (suit and number)
2. String value
3. SVG

### 1.1 Component Data: `getCardAsComponents`

The `getCardAsComponents` function can be used to obtain a breakdown of the card in its component parts - i.e. its suit and number. The function will return two `uint8` values, which can be used in mathematical computations in a smart contract. The values returned represent an array index for the `numbers` and `suits` public arrays respectively. For example, calling&#x20;

```
getCardAsComponents(24);
```

Will return:

```
[6, 0]
```

These return values may be used as they are, of passed to the respective public `numbers` and `suits` arrays to obtain their string values:

```
numbers(6); // returns "8"
suits(0);   // returns "c"
```

### 1.2 String Value: `getCardAsString`

The `getCardAsString` function will simply return the string name of the passed card ID. This may be useful, for example, for displaying quick data about a card. For example,

```
getCardAsString(24);
```

will return `8c`

### 1.3 SVG: `getCardAsSvg`

Finally, the `getCardAsSvg` function will return the given card ID as an SVG XML string. For example

```
getCardAsSvg(24);
```

will return

```
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 72 62" width="2.5in" height="2.147in">
  <symbol id="S0" viewBox="-600 -600 1200 1200">
    <path d="M30 150c5 235 55 250 100 350h-260c45-100 95-115 100-350a10 10 0 0 0-20 0 210 210 0 1 1-74-201 10 10 0 0 0 14-14 230 230 0 1 1 220 0 10 10 0 0 0 14 14 210 210 0 1 1-74 201 10 10 0 0 0-20 0Z" fill="#000"/>
  </symbol>
  <symbol id="F6" viewBox="-600 -600 1200 1200">
    <path d="M-1-50a205 205 0 1 1 2 0h-2a255 255 0 1 0 2 0Z" stroke-width="80" stroke-linecap="square" stroke-miterlimit="1.5" fill="none"/>
  </symbol>
  <rect width="70" height="60" x="1" y="1" rx="6" ry="6" fill="white" stroke="black"/>
  <use xlink:href="#F6" height="32" width="32" x="7" y="16" stroke="#000"/>
  <use xlink:href="#S0" height="32" width="32" x="32" y="16"/>
</svg>
```

which will render as:

![](../.gitbook/assets/24.svg)

## 2. Hand Data

All 1326 starting hands are available to pull into and implement in your smart contracts. As with card data, several options are available for importing and accessing Hand data:

* Hand as Card IDs
* Hand as String
* Hand as SVG

In addition, the following data is also broken down and made available for each hand:

* Hand Name
* Hand Shape
* Hand Rank

### 2.1 Hand as Card IDs: `getHandAsCardIds`

The `getHandAsCardIds` function will return a given hand as its component card IDs, representing `card1` and `card2`. The returned IDs are integer values which can be passed to any of the functions outlined in the Card Data section above. For example

```
getHandAsCardIds(24)
```

Might return `[24, 9]`.&#x20;

### 2.2 Hand as String: `getHandAsString`

The `getHandAsString` function will return the hand data as a concatentation of each card's string name for the given hand ID. For example, `As10c` (Ace of Spades, 10 of Clubs).

### 2.3 Hand as SVG: `getHandAsSvg`

As with the Card variant, the `getHandAsSvg` function will return the SVG XML for the given hand ID, allowing it to be integrated or displayed in a UI for example.

```
getHandAsSvg(24)
```

Might return:

![](<../.gitbook/assets/24 (1).svg>)



### 2.4 Hand Name: `getHandName`

The `getHandName` function will return the canonical Poker name for a given Hand ID, for example `A5s`.

### 2.5 Hand Shape: `getHandShape`

The `getHandShape` function will return the canonical shape for a given Hand ID, for example `Pair`, `Suited` or `Offsuit`.

### 2.6 Hand Rank: `getHandRank`

The `getHandRank` function will return the canonical Poker rank for the given Hand ID. The return value is an integer

## 3. NFT Data

1. Token ID to Hand ID mapping
2. Token URI

### 3.1 Token ID to Hand ID mapping: `tokenIdToHandId`

Once the hands have been revealed and distributed as NFTs, the `tokenIdToHandId` function can be called to map an NFT token ID to its corresponding distributed Hand ID. The returned Hand ID is an integer value that can be used in any of the Hand functions outlined in the section above. The `tokenkIdToHandId` is also called by the `tokenURI` function to return the correct Hand data for the given token ID.

### 3.2 Token URI: `tokenURI`

The first thing to understand about `tokenURI` is that there is no token URI! All of the NFT data, including the JSON metadata, attributes and the image itself is entirely embedded within the Holdem Heroes smart contract. Holdem Heroes does not rely on any external APIs, off-chain storage for its NFT data - it's 100% on chain, so there is no risk of metadata or images ever going offline.

A query to the `tokenURI` function, will output a base64 encoded string containing the full standard NFT JSON, including the image itself as a base64 encoded SVG XML definition. For example:

```
data:application/json;base64,eyJuYW1lIjogIjRzNGQiLCAiZGVzY3JpcHRpb24iOiAiaG9sZGVtaGVyb2VzLmNvbSIsImF0dHJpYnV0ZXMiOiBbeyAidHJhaXRfdHlwZSI6ICJTaGFwZSIsICJ2YWx1ZSI6ICJQYWlyIn0seyAidHJhaXRfdHlwZSI6ICJIYW5kIiwgInZhbHVlIjogIjQ0In0seyAidHJhaXRfdHlwZSI6ICJSYW5rIiwgInZhbHVlIjogIjUwIn1dLCJpbWFnZSI6ICJkYXRhOmltYWdlL3N2Zyt4bWw7YmFzZTY0LFBITjJaeUI0Yld4dWN6MGlhSFIwY0RvdkwzZDNkeTUzTXk1dmNtY3ZNakF3TUM5emRtY2lJSGh0Ykc1ek9uaHNhVzVyUFNKb2RIUndPaTh2ZDNkM0xuY3pMbTl5Wnk4eE9UazVMM2hzYVc1cklpQjJhV1YzUW05NFBTSXdJREFnTVRRNElEWXlJaUIzYVdSMGFEMGlOV2x1SWlCb1pXbG5hSFE5SWpJdU1UUTNhVzRpUGp4emVXMWliMndnYVdROUlsTXpJaUIyYVdWM1FtOTRQU0l0TmpBd0lDMDJNREFnTVRJd01DQXhNakF3SWo0OGNHRjBhQ0JrUFNKTk1DMDFNREJqTVRBd0lESTFNQ0F6TlRVZ05EQXdJRE0xTlNBMk9EVmhNVFV3SURFMU1DQXdJREFnTVMwek1EQWdNQ0F4TUNBeE1DQXdJREFnTUMweU1DQXdZekFnTWpBd0lEVXdJREl4TlNBNU5TQXpNVFZvTFRJMk1HTTBOUzB4TURBZ09UVXRNVEUxSURrMUxUTXhOV0V4TUNBeE1DQXdJREFnTUMweU1DQXdJREUxTUNBeE5UQWdNQ0F3SURFdE16QXdJREJqTUMweU9EVWdNalUxTFRRek5TQXpOVFV0TmpnMVdpSWdabWxzYkQwaUl6QXdNQ0l2UGp3dmMzbHRZbTlzUGp4emVXMWliMndnYVdROUlrWXlJaUIyYVdWM1FtOTRQU0l0TmpBd0lDMDJNREFnTVRJd01DQXhNakF3SWo0OGNHRjBhQ0JrUFNKTk5UQWdORFl3YURJd01HMHRNVEF3SURCMkxUa3lNR3d0TkRVd0lEWXpOWFl5TldnMU56QWlJSE4wY205clpTMTNhV1IwYUQwaU9EQWlJSE4wY205clpTMXNhVzVsWTJGd1BTSnpjWFZoY21VaUlITjBjbTlyWlMxdGFYUmxjbXhwYldsMFBTSXhMalVpSUdacGJHdzlJbTV2Ym1VaUx6NDhMM041YldKdmJENDhjbVZqZENCM2FXUjBhRDBpTnpBaUlHaGxhV2RvZEQwaU5qQWlJSGc5SWpJaUlIazlJakVpSUhKNFBTSTJJaUJ5ZVQwaU5pSWdabWxzYkQwaWQyaHBkR1VpSUhOMGNtOXJaVDBpWW14aFkyc2lMejQ4ZFhObElIaHNhVzVyT21oeVpXWTlJaU5HTWlJZ2FHVnBaMmgwUFNJek1pSWdkMmxrZEdnOUlqTXlJaUI0UFNJM0lpQjVQU0l4TmlJZ2MzUnliMnRsUFNJak1EQXdJaTgrUEhWelpTQjRiR2x1YXpwb2NtVm1QU0lqVXpNaUlHaGxhV2RvZEQwaU16SWlJSGRwWkhSb1BTSXpNaUlnZUQwaU16SWlJSGs5SWpFMklpOCtQSE41YldKdmJDQnBaRDBpVXpFaUlIWnBaWGRDYjNnOUlpMDJNREFnTFRZd01DQXhNakF3SURFeU1EQWlQanh3WVhSb0lHUTlJazB0TkRBd0lEQkRMVE0xTUNBd0lEQXRORFV3SURBdE5UQXdJREF0TkRVd0lETTFNQ0F3SURRd01DQXdJRE0xTUNBd0lEQWdORFV3SURBZ05UQXdJREFnTkRVd0xUTTFNQ0F3TFRRd01DQXdXaUlnWm1sc2JEMGljbVZrSWk4K1BDOXplVzFpYjJ3K1BITjViV0p2YkNCcFpEMGlSaklpSUhacFpYZENiM2c5SWkwMk1EQWdMVFl3TUNBeE1qQXdJREV5TURBaVBqeHdZWFJvSUdROUlrMDFNQ0EwTmpCb01qQXdiUzB4TURBZ01IWXRPVEl3YkMwME5UQWdOak0xZGpJMWFEVTNNQ0lnYzNSeWIydGxMWGRwWkhSb1BTSTRNQ0lnYzNSeWIydGxMV3hwYm1WallYQTlJbk54ZFdGeVpTSWdjM1J5YjJ0bExXMXBkR1Z5YkdsdGFYUTlJakV1TlNJZ1ptbHNiRDBpYm05dVpTSXZQand2YzNsdFltOXNQanh5WldOMElIZHBaSFJvUFNJM01DSWdhR1ZwWjJoMFBTSTJNQ0lnZUQwaU56WWlJSGs5SWpFaUlISjRQU0kySWlCeWVUMGlOaUlnWm1sc2JEMGlkMmhwZEdVaUlITjBjbTlyWlQwaVlteGhZMnNpTHo0OGRYTmxJSGhzYVc1ck9taHlaV1k5SWlOR01pSWdhR1ZwWjJoMFBTSXpNaUlnZDJsa2RHZzlJak15SWlCNFBTSTRNaUlnZVQwaU1UWWlJSE4wY205clpUMGljbVZrSWk4K1BIVnpaU0I0YkdsdWF6cG9jbVZtUFNJalV6RWlJR2hsYVdkb2REMGlNeklpSUhkcFpIUm9QU0l6TWlJZ2VEMGlNVEEzSWlCNVBTSXhOaUl2UGp3dmMzWm5QZz09In0=
```

When decoded, this yields:

```
{
  "name": "4s4d",
  "description": "holdemheroes.com",
  "attributes": [
    {
      "trait_type": "Shape",
      "value": "Pair"
    },
    {
      "trait_type": "Hand",
      "value": "44"
    },
    {
      "trait_type": "Rank",
      "value": "50"
    }
  ],
  "image": "data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2aWV3Qm94PSIwIDAgMTQ4IDYyIiB3aWR0aD0iNWluIiBoZWlnaHQ9IjIuMTQ3aW4iPjxzeW1ib2wgaWQ9IlMzIiB2aWV3Qm94PSItNjAwIC02MDAgMTIwMCAxMjAwIj48cGF0aCBkPSJNMC01MDBjMTAwIDI1MCAzNTUgNDAwIDM1NSA2ODVhMTUwIDE1MCAwIDAgMS0zMDAgMCAxMCAxMCAwIDAgMC0yMCAwYzAgMjAwIDUwIDIxNSA5NSAzMTVoLTI2MGM0NS0xMDAgOTUtMTE1IDk1LTMxNWExMCAxMCAwIDAgMC0yMCAwIDE1MCAxNTAgMCAwIDEtMzAwIDBjMC0yODUgMjU1LTQzNSAzNTUtNjg1WiIgZmlsbD0iIzAwMCIvPjwvc3ltYm9sPjxzeW1ib2wgaWQ9IkYyIiB2aWV3Qm94PSItNjAwIC02MDAgMTIwMCAxMjAwIj48cGF0aCBkPSJNNTAgNDYwaDIwMG0tMTAwIDB2LTkyMGwtNDUwIDYzNXYyNWg1NzAiIHN0cm9rZS13aWR0aD0iODAiIHN0cm9rZS1saW5lY2FwPSJzcXVhcmUiIHN0cm9rZS1taXRlcmxpbWl0PSIxLjUiIGZpbGw9Im5vbmUiLz48L3N5bWJvbD48cmVjdCB3aWR0aD0iNzAiIGhlaWdodD0iNjAiIHg9IjIiIHk9IjEiIHJ4PSI2IiByeT0iNiIgZmlsbD0id2hpdGUiIHN0cm9rZT0iYmxhY2siLz48dXNlIHhsaW5rOmhyZWY9IiNGMiIgaGVpZ2h0PSIzMiIgd2lkdGg9IjMyIiB4PSI3IiB5PSIxNiIgc3Ryb2tlPSIjMDAwIi8+PHVzZSB4bGluazpocmVmPSIjUzMiIGhlaWdodD0iMzIiIHdpZHRoPSIzMiIgeD0iMzIiIHk9IjE2Ii8+PHN5bWJvbCBpZD0iUzEiIHZpZXdCb3g9Ii02MDAgLTYwMCAxMjAwIDEyMDAiPjxwYXRoIGQ9Ik0tNDAwIDBDLTM1MCAwIDAtNDUwIDAtNTAwIDAtNDUwIDM1MCAwIDQwMCAwIDM1MCAwIDAgNDUwIDAgNTAwIDAgNDUwLTM1MCAwLTQwMCAwWiIgZmlsbD0icmVkIi8+PC9zeW1ib2w+PHN5bWJvbCBpZD0iRjIiIHZpZXdCb3g9Ii02MDAgLTYwMCAxMjAwIDEyMDAiPjxwYXRoIGQ9Ik01MCA0NjBoMjAwbS0xMDAgMHYtOTIwbC00NTAgNjM1djI1aDU3MCIgc3Ryb2tlLXdpZHRoPSI4MCIgc3Ryb2tlLWxpbmVjYXA9InNxdWFyZSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEuNSIgZmlsbD0ibm9uZSIvPjwvc3ltYm9sPjxyZWN0IHdpZHRoPSI3MCIgaGVpZ2h0PSI2MCIgeD0iNzYiIHk9IjEiIHJ4PSI2IiByeT0iNiIgZmlsbD0id2hpdGUiIHN0cm9rZT0iYmxhY2siLz48dXNlIHhsaW5rOmhyZWY9IiNGMiIgaGVpZ2h0PSIzMiIgd2lkdGg9IjMyIiB4PSI4MiIgeT0iMTYiIHN0cm9rZT0icmVkIi8+PHVzZSB4bGluazpocmVmPSIjUzEiIGhlaWdodD0iMzIiIHdpZHRoPSIzMiIgeD0iMTA3IiB5PSIxNiIvPjwvc3ZnPg=="
}
```

