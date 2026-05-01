Searching for stuff on Scryfall can be kind of difficult, so we've decided to write a little guide for it. This assumes you're familiar with the basics -- naive name matches, how to search different fields. [The syntax guide](https://scryfall.com/docs/syntax) is your friend, though it's incomplete.  

Some sample searches. For all sample searches, replace %s with your actual search query.
Search through only reminder text: `fo:/\(.*%s.*\)/`.  
Find all DFCs which are exactly one colour on each side: `is:dfc -c>1 id=2 -c=0`. This doesn't exactly work -- you don't have capture groups, so it's impossible within just Scryfall -- but it gets quite close.  

Scryfall golf is a fun little game we enjoy playing. 

Some very low-character matches, none of which I've ever gotten in a real round:  
`4` -- [T-45 Power Armor + Land Aid '04](https://scryfall.com/search?q=4&unique=cards&as=grid&order=name)  
`5` -- [T-45 Power Armor + Vault 75: Middle School](https://scryfall.com/search?q=5&unique=cards&as=grid&order=name)  
`6` -- [Overseer of Vault 76 + T-60 Power Armor](https://scryfall.com/search?q=6&unique=cards&as=grid&order=name)  
`99` -- [Spider-Man 2099 + Spider-Man 2099, Miguel O'Hara](https://scryfall.com/search?q=99&unique=cards&as=grid&order=name)  
This isn't very interesting, I'm just showcasing lower bounds.  

There's a large collection of seven-character, single-field solutions. The set CMM has collector numbers from 1 to 1067, the set WHO has collector numbers from 1 to 1165, and the set PIP has collector numbers from 1 to 1056. There's also the set SLD, which has collector numbers into the thousands, but has a lot of gaps. And there's also promos, which aren't really relevant.  
This means that there's often collector numbers only two cards share. An example is all of 1069 through 1084. 1085 contains a card from The List, though, and as such isn't valid. I don't know how many of these there are, I just think it's neat there are a lot of them.  
