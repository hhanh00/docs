---
title: HG API
---

## Terminology 

The key words "MUST", "SHOULD", "NOT RECOMMENDED", and "MAY" in this document are to be interpreted as described in BCP 14 1 when, and only when, they appear in all capitals.

# Transport

FE to BE communicate using a websocket connection
and use JSON for message serialization

# FE -> BE

## Send Prompt

```json
{
    "prompt": "Who plays Katniss?"
}
```

## Reengagement

After a certain time, the FE will send a request
to get user information. It signals that the BE
should reply with questions such as: "What was
your favorite movie of the franchise?"

```json
{
    "ext": "reengage"
}
```

# BE -> FE

```json
{
  "data": "Jennifer Lawrence brings to life the character of Katniss Everdeen in \"The Hunger Games\" film series, captivating audiences with her powerful portrayal. Known for her resilience and sharp survival skills, Katniss becomes a symbol of hope and rebellion against oppression. Lawrence's performance has been pivotal, earning acclaim for her depth and authenticity in the role.<p>[[Asset-0]]<p>Preparing for Katniss required Lawrence to undergo intense physical training and archery lessons, immersing herself in the character's world. This preparation helped her convincingly navigate the physical and emotional landscapes of the dystopian setting, from the perilous Hunger Games arena to the complexities of her relationships.<p>Jennifer Lawrence's role as Katniss Everdeen not only showcased her acting range but also significantly boosted her career, establishing her as a leading actress in Hollywood. Her portrayal resonates with fans for its strength and vulnerability, making Katniss a memorable and inspiring character.",
  "assets": [
    {
      "url": "https://en.wikipedia.org/wiki/Katniss_Everdeen#/media/File:Katniss_Everdeen.jpg",
      "title": "Katniss Everdeen, as portrayed by Jennifer Lawrence in the film the Hunger Games"
    }
  ],
  "followup": [
    "How did Jennifer Lawrence prepare for the role of Katniss Everdeen?",
    "What were the challenges Jennifer Lawrence faced while filming \"The Hunger Games\"?"
  ]
}
```

1. Paragraphs SHOULD begin with `<p>`. (The first paragraph MAY not have it)
1. Assets MUST be images or videos
1. References to assets MUST be `[[Asset-#]]`. The `#` is the
index of the asset in the `assets` array
1. Assets MUST have a url
1. Assets SHOULD have a title
1. Follow up MUST be simple text (no markup)

# Content Feed

The first reply from the server MUST be the content feed.
It SHOULD have no data.

# Sequence

```mermaid
sequenceDiagram
    FE->>BE: WS Connect
    BE-->>FE: Content Feed
    loop repeat
    FE->>BE: Prompt
    BE-->>FE: Reply
    end
    FE->>BE: Reengage (When 30s elapsed without interaction)
    BE-->>FE: Prompt
```
