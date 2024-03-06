---
title: 'Coming Back to Poker - Part 0'
description: 'Backgrround - my first attempt to create poker with python'
pubDate: 'Aug 19 2023'
heroImage: '/blog-placeholder-3.jpg'
---

## Background

When I first started learning python, way back in 2018, the first properly ambitious project I attempted was to create poker, specifically Texas Hold'Em. I had just done a bunch of tutorials, and then applied that knowledge by creating a simple implementation of Connect 4 as a terminal application, so I was feeling pretty confident at the time.

Well it turns out that poker is a fairly big step up in terms of complexity from connect 4, and with my now obvious lack of experience in software design principals and best practices, it wasn't long before I had made some serious spagghetti code. I mean look at this.

```python
def pairstraightdetect(allranks,faceranks,numnames):
    ranklist=list(range(2,15))
    for i in range(0,len(allranks)):
        ranks=allranks[i]
        numranks=[]
        for x in ranklist:
            if x <= 10:
                numrank = ranks.count(str(x))
            else:
                numrank = ranks.count(faceranks[x])
            numranks.append(numrank)

        #Look for 2, 3 and 4 of a kind, as well as full house and 2 pairs

        #Redefine faceranks to have the whole word, i.e. ace
        faceranks[14]='ace'
        faceranks[13]='king'
        faceranks[12]='queen'
        faceranks[11]='jack'

        ispair=any(y == 2 for y in numranks)
        if ispair == True:
            for j in range(0,len(numranks)):
                if numranks[j]==2:
                    pairrank = str(j+2)
                    if int(pairrank) > 10:
                        pairrank = faceranks[int(pairrank)]
                
        if numranks.count(2) == 2:
            is2pair=True
        else:
            is2pair=False
        if is2pair == True:
            for j in range(0,len(numranks)):
                firstpair = False
                if numranks[j] == 2 and firstpair == False:
                    pair1rank = str(j+2)
                    firstpair = True
                    if int(pair1rank) > 10:
                        pair1rank = faceranks[int(pair1rank)]
                if numranks[j] == 2 and j+2 != int(pair1rank):
                    pair2rank = str(j+2)
                    if int(pair2rank) > 10:
                        pair2rank = faceranks[int(pair2rank)]
                        
                

        is3kind=any(y == 3 for y in numranks)
        if is3kind == True:
            for j in range(0,len(numranks)):
                if numranks[j]==3:
                    threekindrank = str(j+2)
                    if int(threekindrank) > 10:
                        threekindrank = faceranks[int(threekindrank)]
                        
        is4kind=any(y == 4 for y in numranks)
        if is4kind == True:
            for j in range(0,len(numranks)):
                if numranks[j]==4:
                    fourkindrank = str(j+2)
                    if int(fourkindrank) > 10:
                        fourkindrank = faceranks[int(fourkindrank)]
        
        isfullhouse = ispair and is3kind
        if is4kind == True:
            print(numnames[i] + ' has 4 of a kind of ' + fourkindrank + 's.')
        else:
            if isfullhouse == True:
                print(numnames[i] + ' has a full house.')
            else:
                if is3kind == True:
                    print(numnames[i] + ' has 3 of a kind of ' + threekindrank + 's.')
                else:
                    if is2pair == True:
                       print(numnames[i] + ' has 2 pairs: a pair of ' + pair2rank + 's and a pair of ' + pair1rank + 's.')
                    else:
                        if ispair == True:
                            print(numnames[i] + ' has a pair of ' + pairrank + 's.')
       

        #Look for straights
        for y in range(0,len(numranks)):
            if numranks[y] > 1:
                numranks[y]=1
        sequences=[len(list(g[1])) for g in groupby(numranks) if g[0] == 1]
        isstraight=any(y >= 5 for y in sequences)
        if isstraight == True:
            print(numnames[i] + ' has a straight.')
        else:
            numranksdq=deque(numranks)
            numranksdq.rotate(-1)
            numranks=list(numranksdq)
            sequences=[len(list(g[1])) for g in groupby(numranks) if g[0] == 1]
            isstraight=any(y >= 5 for y in sequences)
            if isstraight == True:
                print(numnames[i] + ' has a straight.')
```

Well done for finally scrolling past that plate of spagghetti. Now, 5 years later, I'm coming back and trying to make poker again, but applying everything I've learned on my programming journey so far. This should be a fun adventure!

P.S. of COURSE I had to call this post part 0, because programming.
