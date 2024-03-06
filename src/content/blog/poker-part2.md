---
title: 'Coming Back to Poker - Part 2'
description: 'The fun part: hand evaluation'
pubDate: 'Aug 21 2023'
heroImage: '/blog-placeholder-4.jpg'
---

## The Hand Checker interface

This was a great usecase I found for a functional interface

```typescript
export interface HandChecker {
    (cards: Card[]): number | undefined
}
```

The idea here is that for each type of hand (flush, pair, straight etc.), I would define a ```HandChecker```, a function that would return a number if the hand is present, and undefined otherwise. The number then represents the value of that hand relative to itself. For example the rank of the highest card in a straight or the rank of a three of a kind. For example, here is the ```HandChecker``` for a flush:

```typescript
export const findPotentialFlush: HandChecker = (cards: Card[]): number | undefined => {
    const numForFlush = 5;
    return [ ...cards ]
        .sort((c1, c2) => c2.rank - c1.rank)
        .find(c => countCardsOfSuit(c.suit, cards) >= numForFlush)?.rank;
};
```

Of course I had to keep things DRY, so I made a generic ```findPotentialGroup```  function which could be used for pairs, three of a kinds and four of a kinds.

```typescript
export const findPotentialGroup = (cards: Card[], groupSize: number): number | undefined => {
    // If group(s) found, return the rank of the highest ranking group, otherwise return undefined
    return [ ...cards.map(c => c.rank) ]
        .sort((r1, r2) => r2 - r1)
        .find(r => countCardsOfRank(r, cards) === groupSize);
};
```

The beauty of using this interface is that the hand checkers are composeable and flexible. What if I want to try a variant of poker without straights? Or where three of a kind is worth more than a flush? No problem, I'd just need to pass in a different listing of HandCheckers to the findBestHand function.

```typescript
export const findBestHand = (cards: Card[], handCheckers: HandChecker[]): HandEvaluation => {
    const succesfulChecker = handCheckers.find(hc => hc(cards));
    if (!succesfulChecker) {
        throw new Error("Error evaluating hand");
    }
    const handValue = succesfulChecker(cards) as number;
    return {
        handRank: handCheckers.indexOf(succesfulChecker),
        handValue
    };
};
```
