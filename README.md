# Meeting Intelligence

En tre-stations ALP-linje til Agentics Fabrik. Når linjen importeres fra
Marketplace, opretter Fabrikken en dedikeret Agent Inbox-adresse. Send en Teams-
eller Google Meet-invitation til adressen; mødeagenten joiner og optager, og når
mødet slutter starter linjen automatisk.

Importeres i et projekt på agentics.dk ved at pege på dette repo.

## Tre stationer

| Station | Hvad den gør | Hvorfor den er sin egen |
|---|---|---|
| `transcribe` | Den live-captured transcript → `TRANSCRIPT.md` + `meeting.json` | Bevarer evidensen og markerer huller uden at gætte. |
| `analyze` | Transcript → `ANALYSIS.md` | Finder beslutninger, actions, risici og mødemønstre med timestamps. |
| `publish` | Analyse + transcript → `MEETING.md` | Leverer det korte, handlingsklare mødeartefakt. |

At holde dem adskilt er ikke pænhed. Transskriptionen kan verificeres mod optagelsen;
referatet kan ikke. Blandes de sammen, kan man ikke længere se hvor det målte holder op
og det vurderede begynder — og så bliver hele mødemappen lige så meget værd som den
svageste halvdel.

## Factory-krav

- Agent Inbox
- Agent Meeting
- En online ALP-runner med adgang til en aktiveret model

## Sådan bruges den

Importer linjen fra Marketplace, vælg inbox-navn og det navn mødeagenten skal vise i
deltagerlisten, og invitér den viste emailadresse til et møde.

Resultatet er en mappe:

```
TRANSCRIPT.md
meeting.json
ANALYSIS.md
MEETING.md
```

Mødeservicen leverer allerede en live transcript sammen med optagelsen. Derfor er
første station en verificerende/normaliserende transcript-station. En senere version
kan lave en separat audio re-transcription, når runnerens medieinput er bærbart.
