# alp-meeting-actions

En ALP-samlebåndslinje der tager en mødeoptagelse og laver den til en mødemappe:
et ordret transskript med talere, og et referat med aftalte actions, ejer og
tidsstempel.

Importeres i et projekt på agentics.dk ved at pege på dette repo.

## To stationer, med vilje

| Station | Hvad den gør | Hvorfor den er sin egen |
|---|---|---|
| `transcribe` | Optagelse → `transcript.md` / `.json` / `.srt` + `meeting.json` | Mekanisk. Værktøjet måler; der er ikke noget at være uenig om. |
| `actions` | Transskript → `README.md` med aftalte actions | Vurdering. Hvad der *blev aftalt* står ikke i lyden — det læses ud af den. |

At holde dem adskilt er ikke pænhed. Transskriptionen kan verificeres mod optagelsen;
referatet kan ikke. Blandes de sammen, kan man ikke længere se hvor det målte holder op
og det vurderede begynder — og så bliver hele mødemappen lige så meget værd som den
svageste halvdel.

## Krav

- **`pks-cli`** på runneren (`dotnet tool install -g pks-cli`). Den ejer
  transskriptionen — to motorer, sammenflettet ord for ord — og formatet på mødemappen:
  [`docs/meeting-folder-format.md`](https://github.com/pksorensen/pks-cli/blob/main/docs/meeting-folder-format.md).
- **Adgang til en model-provider** for `pks transcribe`. Det er den del der i dag
  binder linjen til en runner hvor den adgang allerede er sat op — se begrænsningerne
  nedenfor.
- **Optagelsen skal ligge på runneren.** Den uploades ikke nogen steder hen.

## Sådan bruges den

Opret en opgave med stien til optagelsen:

```
meetings/2026-01-15-statusmoede/raw/recording.mp4
Talere: 1=Fornavn, 2=Fornavn
```

Talerne må gerne udelades — første station kører `pks diarize` og kan som regel selv
binde label til navn ud fra taletid og de første minutter. Er den i tvivl, hedder de
`Taler 1` og `Taler 2`, og det er det ærlige udfald.

Resultatet er en mappe:

```
2026-01-15-statusmoede/
├─ README.md          ← station 2: resumé + aftalte actions
├─ meeting.json       ← manifest: metode, providere, talere, labelScope
├─ transcript.md      ← læsbar, taler + tidsstempel
├─ transcript.json
├─ transcript.srt
└─ raw/
   ├─ recording.mp4   ← git-ignoreret
   ├─ engine/         ← rå motorsvar; gør sammenfletningen replayable gratis
   └─ turns.jsonl
```

## Begrænsninger, sagt højt

**Input leveres i opgavebeskrivelsen.** Linjen henter ikke selv en optagelse fra en
kalender, en Teams-optagelse eller et arkiv. Nogen lægger filen i repoet, eller
optagelsen ligger der allerede fra en anden proces.

**Model-adgang er ikke bærbar.** `pks transcribe` skal kunne nå sin provider, og den
konfiguration bor på runneren — ikke i denne linje. På en vilkårlig runner uden den
opsætning stopper station 1 med det samme og siger hvorfor. Det er det rigtige udfald;
det er bare ikke selvbetjening endnu.

Begge dele er kendte og ligger til at blive løst hver for sig. Ingen af dem er
skjulte fejl — linjen stopper med en forklaring i stedet for at gætte.
