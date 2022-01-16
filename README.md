# wordle3

Program to solve for the 2022-1-14 Riddler Classic found at [When The Riddler Met Wordle](https://fivethirtyeight.com/features/when-the-riddler-met-wordle/), based on the web game [Wordle](https://www.powerlanguage.co.uk/wordle/).

Runnable playground [here](https://htmlpreview.github.io/?https://github.com/nasderidaq/wordle3/blob/master/wordle.html) (use the console and the `window.wordle` exports).

## Strategy

This JavaScript program performs an exhaustive tree search given the list of possible words, constantly narrowing down the valid remaining answers. The search spawns a number of threads equal to the number of available cores, and divies up the root words to search amongst them. Go dumb parallelism, go!

There are a few optimizations at play:
 * If we are on our last (3rd) guess, we know the best we can do is get one correct answer by guessing one of the valid remaining answers
 * If have only 1 or 2 valid remaining answers, just guess one of them (we know this is optimal)
 * We keep track of the best score we have, and use that as a "minimum" while evaluating each position, and we bail out early if we know we cannot achieve that minimum
 * If we see only a single possible feedback result for all valid remaining answers, then we bail because we guessed a word that gave us no new information
 * If we are on our penultimate (2nd) guess, we know the best we can possibly do is equal to the highest number of feedback entries for any word compared against all valid answers, which happens to be 150.

Barring any bugs (🤷), the result is guaranteed to be optimal. For me, the full search took ~10 hours running on 8 cores.

## Results

The answer to the question posed in the Riddler appears to be `trace`, resulting in 1388 success for a rate of 59.95% (1 word shy of exactly 60%).

| word  | score | by # guesses | feedback | word  | score | by # guesses |
|-------|-------|--------------|----------|-------|-------|--------------|
| trace | 1388  | 1, 75, 1312  | 🟩🟩🟩🟩🟩    |       |       |              |
|       |       |              | 🟨🟩🟨⬜⬜    | artsy | 1     | 1            |
|       |       |              | ⬜🟨🟨🟨🟩    | carve | 1     | 1            |
|       |       |              | 🟨🟨🟨🟨🟨    | cater | 1     | 1            |
|       |       |              | 🟨🟨🟩🟨⬜    | chart | 1     | 1            |
|       |       |              | 🟨🟨⬜🟨⬜    | court | 1     | 1            |
|       |       |              | 🟨🟩🟩🟨⬜    | craft | 1     | 1            |
|       |       |              | 🟨🟩🟩🟨🟩    | crate | 1     | 1            |
|       |       |              | ⬜🟩🟨🟨⬜    | croak | 1     | 1            |
|       |       |              | 🟨🟩⬜🟩🟨    | erect | 1     | 1            |
|       |       |              | ⬜🟨🟨🟩🟩    | farce | 1     | 1            |
|       |       |              | ⬜🟨⬜🟩🟩    | force | 1     | 1            |
|       |       |              | 🟨🟩🟨⬜🟨    | great | 1     | 1            |
|       |       |              | 🟨🟨🟩⬜🟨    | heart | 1     | 1            |
|       |       |              | ⬜⬜🟨🟩🟨    | mecca | 1     | 1            |
|       |       |              | ⬜🟩⬜🟩🟩    | price | 1     | 1            |
|       |       |              | ⬜🟨🟩🟩🟨    | reach | 1     | 1            |
|       |       |              | 🟨🟨🟩🟩🟨    | react | 1     | 1            |
|       |       |              | 🟨🟨⬜🟨🟨    | recut | 1     | 1            |
|       |       |              | 🟨🟨⬜🟩🟨    | retch | 1     | 1            |
|       |       |              | ⬜🟨🟩🟩⬜    | roach | 1     | 1            |
|       |       |              | ⬜🟨🟩🟨🟩    | scare | 1     | 1            |
|       |       |              | 🟨⬜🟩🟩⬜    | stack | 1     | 1            |
|       |       |              | 🟨🟨🟩⬜🟩    | stare | 1     | 1            |
|       |       |              | 🟩⬜🟩🟩🟨    | teach | 1     | 1            |
|       |       |              | 🟩🟨🟩⬜🟨    | teary | 1     | 1            |
|       |       |              | 🟩⬜🟩⬜🟩    | tease | 1     | 1            |
|       |       |              | 🟩🟨🟩⬜⬜    | tiara | 1     | 1            |
|       |       |              | 🟩🟨⬜🟩⬜    | torch | 1     | 1            |
|       |       |              | 🟩🟩🟩⬜🟩    | trade | 1     | 1            |
|       |       |              | 🟩⬜⬜🟩🟩    | twice | 1     | 1            |
|       |       |              | ⬜🟩⬜🟩🟨    | wreck | 1     | 1            |
|       |       |              | 🟨🟨🟨🟨⬜    | actor | 2     | 1, 1         |
|       |       |              | 🟨⬜🟨🟨🟩    | acute | 2     | 1, 1         |
|       |       |              | ⬜🟩🟩🟩🟩    | brace | 2     | 1, 1         |
|       |       |              | 🟨⬜⬜🟨🟩    | chute | 2     | 1, 1         |
|       |       |              | ⬜🟩🟩🟩⬜    | crack | 2     | 1, 1         |
|       |       |              | ⬜🟩🟨🟨🟨    | creak | 2     | 1, 1         |
|       |       |              | 🟨🟩⬜🟨🟨    | crept | 2     | 1, 1         |
|       |       |              | 🟨🟩⬜🟨⬜    | crust | 2     | 1, 1         |
|       |       |              | 🟨⬜🟩🟩🟨    | enact | 2     | 1, 1         |
|       |       |              | ⬜⬜🟨🟩⬜    | fancy | 2     | 1, 1         |
|       |       |              | 🟨🟩🟩⬜🟩    | grate | 2     | 1, 1         |
|       |       |              | ⬜🟨⬜🟩🟨    | mercy | 2     | 1, 1         |
|       |       |              | 🟩⬜🟨⬜🟩    | table | 2     | 1, 1         |
|       |       |              | 🟩⬜🟨🟨⬜    | tacit | 2     | 1, 1         |
|       |       |              | 🟩⬜⬜🟩⬜    | thick | 2     | 1, 1         |
|       |       |              | 🟩🟩🟩🟩⬜    | track | 2     | 1, 1         |
|       |       |              | 🟩🟩🟨⬜🟨    | tread | 2     | 1, 1         |
|       |       |              | 🟩🟩🟨⬜⬜    | triad | 2     | 1, 1         |
|       |       |              | 🟩🟩⬜🟩🟩    | trice | 2     | 1, 1         |
|       |       |              | 🟩🟩⬜🟩⬜    | trick | 2     | 1, 1         |
|       |       |              | ⬜🟩🟨⬜🟩    | arise | 3     | 1, 2         |
|       |       |              | ⬜⬜🟩🟩🟨    | abled | 3     | 0, 3         |
|       |       |              | ⬜🟨⬜🟩⬜    | abbot | 3     | 0, 3         |
|       |       |              | 🟨🟩⬜⬜🟩    | write | 3     | 1, 2         |
|       |       |              | 🟨⬜🟩🟨⬜    | chant | 3     | 1, 2         |
|       |       |              | ⬜🟨🟨🟩⬜    | circa | 3     | 1, 2         |
|       |       |              | ⬜🟩🟩🟨🟩    | anvil | 3     | 0, 3         |
|       |       |              | ⬜⬜🟨🟩🟩    | dance | 3     | 1, 2         |
|       |       |              | 🟨🟩⬜⬜🟨    | greet | 3     | 1, 2         |
|       |       |              | 🟨🟨⬜⬜🟩    | forte | 3     | 1, 2         |
|       |       |              | ⬜⬜🟩🟩🟩    | peace | 3     | 1, 2         |
|       |       |              | 🟩⬜🟨⬜🟨    | taken | 3     | 1, 2         |
|       |       |              | 🟩🟨🟨⬜⬜    | tardy | 3     | 1, 2         |
|       |       |              | 🟩🟨⬜⬜🟩    | terse | 3     | 1, 2         |
|       |       |              | 🟩⬜🟩⬜⬜    | thank | 3     | 1, 2         |
|       |       |              | 🟩🟩⬜⬜🟨    | trend | 3     | 1, 2         |
|       |       |              | ⬜⬜🟨🟨🟩    | abhor | 4     | 0, 4         |
|       |       |              | ⬜⬜🟩🟨🟩    | cease | 4     | 1, 3         |
|       |       |              | 🟨🟩🟩⬜⬜    | draft | 4     | 1, 3         |
|       |       |              | 🟩🟨🟨⬜🟨    | impel | 4     | 0, 4         |
|       |       |              | 🟩⬜⬜🟨⬜    | apron | 4     | 0, 4         |
|       |       |              | ⬜🟩⬜🟩⬜    | cabin | 5     | 0, 5         |
|       |       |              | 🟨⬜🟨🟨🟨    | cheat | 5     | 1, 4         |
|       |       |              | ⬜🟨🟨🟨🟨    | cedar | 5     | 1, 4         |
|       |       |              | ⬜🟨🟩🟨⬜    | daisy | 5     | 0, 5         |
|       |       |              | ⬜🟨⬜🟨🟩    | curse | 5     | 1, 4         |
|       |       |              | ⬜🟩⬜🟨🟩    | admin | 5     | 0, 5         |
|       |       |              | 🟨⬜⬜🟩🟨    | bevel | 5     | 0, 5         |
|       |       |              | 🟩🟩⬜⬜🟩    | bicep | 5     | 0, 5         |
|       |       |              | ⬜🟩🟩🟨⬜    | amyls | 6     | 0, 6         |
|       |       |              | 🟩🟩🟩⬜⬜    | islet | 6     | 0, 6         |
|       |       |              | 🟨🟨🟩⬜⬜    | skimp | 7     | 0, 7         |
|       |       |              | ⬜🟩🟨⬜🟨    | bread | 6     | 1, 5         |
|       |       |              | ⬜🟨🟩⬜🟩    | align | 5     | 0, 5         |
|       |       |              | 🟨⬜🟨🟩⬜    | blimp | 5     | 0, 5         |
|       |       |              | 🟨⬜⬜🟨🟨    | moist | 7     | 0, 7         |
|       |       |              | 🟩🟨⬜⬜🟨    | bigot | 7     | 0, 7         |
|       |       |              | 🟨⬜🟨🟨⬜    | nutty | 8     | 0, 8         |
|       |       |              | ⬜⬜🟩⬜🟨    | child | 7     | 0, 7         |
|       |       |              | 🟨⬜⬜🟨⬜    | clout | 8     | 1, 7         |
|       |       |              | ⬜🟩⬜🟨🟨    | plied | 8     | 0, 8         |
|       |       |              | ⬜🟨🟨⬜🟩    | align | 8     | 0, 8         |
|       |       |              | ⬜🟨🟩⬜🟨    | badly | 8     | 0, 8         |
|       |       |              | 🟨🟨🟨⬜🟨    | easel | 9     | 0, 9         |
|       |       |              | 🟨⬜🟨⬜🟩    | blush | 9     | 0, 9         |
|       |       |              | ⬜⬜⬜🟩🟨    | bleep | 10    | 0, 10        |
|       |       |              | ⬜🟩🟩⬜🟩    | damps | 7     | 0, 7         |
|       |       |              | ⬜⬜🟨🟨🟨    | olden | 10    | 0, 10        |
|       |       |              | 🟩🟩⬜⬜⬜    | lupus | 9     | 0, 9         |
|       |       |              | ⬜🟩⬜🟨⬜    | spunk | 11    | 0, 11        |
|       |       |              | 🟩⬜⬜⬜🟩    | shell | 10    | 0, 10        |
|       |       |              | 🟩🟨⬜⬜⬜    | butoh | 11    | 0, 11        |
|       |       |              | ⬜⬜🟩🟩⬜    | blush | 10    | 0, 10        |
|       |       |              | 🟨⬜🟩⬜🟩    | glops | 9     | 0, 9         |
|       |       |              | 🟨🟨🟨⬜⬜    | worry | 11    | 0, 11        |
|       |       |              | 🟨⬜🟩⬜🟨    | sadly | 9     | 0, 9         |
|       |       |              | 🟨⬜⬜🟩⬜    | hound | 11    | 0, 11        |
|       |       |              | ⬜🟨🟨🟨⬜    | carom | 12    | 0, 12        |
|       |       |              | ⬜🟩🟨⬜⬜    | arbor | 11    | 1, 10        |
|       |       |              | ⬜⬜⬜🟨🟨    | coley | 12    | 0, 12        |
|       |       |              | 🟨🟩⬜⬜⬜    | bonus | 12    | 0, 12        |
|       |       |              | ⬜🟨⬜🟨🟨    | crier | 11    | 0, 11        |
|       |       |              | 🟩⬜⬜⬜🟨    | mened | 13    | 0, 13        |
|       |       |              | ⬜⬜⬜🟩🟩    | noups | 12    | 0, 12        |
|       |       |              | ⬜⬜⬜🟨🟩    | chons | 15    | 0, 15        |
|       |       |              | ⬜🟨🟩⬜⬜    | dhows | 13    | 0, 13        |
|       |       |              | ⬜⬜🟩🟨⬜    | blips | 15    | 0, 15        |
|       |       |              | 🟩⬜⬜⬜⬜    | dying | 14    | 0, 14        |
|       |       |              | 🟨⬜🟨⬜🟨    | pleat | 14    | 1, 13        |
|       |       |              | ⬜🟩⬜⬜🟨    | defer | 15    | 0, 15        |
|       |       |              | ⬜🟨⬜🟨⬜    | choir | 16    | 1, 15        |
|       |       |              | 🟨⬜⬜⬜🟩    | muils | 16    | 0, 16        |
|       |       |              | 🟨⬜🟩⬜⬜    | slink | 16    | 0, 16        |
|       |       |              | 🟩⬜🟨⬜⬜    | noily | 17    | 0, 17        |
|       |       |              | ⬜🟩⬜⬜🟩    | ponds | 16    | 0, 16        |
|       |       |              | ⬜🟩🟩⬜⬜    | blind | 18    | 0, 18        |
|       |       |              | 🟨🟨⬜⬜🟨    | metes | 19    | 0, 19        |
|       |       |              | ⬜⬜🟨🟨⬜    | monal | 22    | 0, 22        |
|       |       |              | 🟨🟨⬜⬜⬜    | shout | 21    | 0, 21        |
|       |       |              | ⬜⬜🟩⬜🟩    | plush | 18    | 0, 18        |
|       |       |              | ⬜⬜⬜🟩⬜    | cuish | 24    | 0, 24        |
|       |       |              | ⬜🟨⬜⬜🟩    | sough | 23    | 0, 23        |
|       |       |              | ⬜🟨🟨⬜🟨    | palsy | 18    | 0, 18        |
|       |       |              | ⬜⬜🟨⬜🟩    | lambs | 23    | 0, 23        |
|       |       |              | ⬜⬜🟨⬜🟨    | kneel | 27    | 0, 27        |
|       |       |              | ⬜⬜⬜🟨⬜    | muils | 33    | 0, 33        |
|       |       |              | ⬜⬜🟩⬜⬜    | shuln | 31    | 0, 31        |
|       |       |              | ⬜🟩⬜⬜⬜    | guild | 27    | 0, 27        |
|       |       |              | 🟨⬜🟨⬜⬜    | spiny | 26    | 0, 26        |
|       |       |              | 🟨⬜⬜⬜🟨    | peels | 32    | 0, 32        |
|       |       |              | ⬜🟨🟨⬜⬜    | pylon | 29    | 0, 29        |
|       |       |              | ⬜🟨⬜⬜⬜    | sound | 34    | 0, 34        |
|       |       |              | ⬜⬜⬜⬜🟩    | sling | 46    | 0, 46        |
|       |       |              | ⬜🟨⬜⬜🟨    | sinew | 34    | 0, 34        |
|       |       |              | 🟨⬜⬜⬜⬜    | hoist | 45    | 1, 44        |
|       |       |              | ⬜⬜⬜⬜🟨    | lined | 49    | 0, 49        |
|       |       |              | ⬜⬜🟨⬜⬜    | sonly | 52    | 0, 52        |
|       |       |              | ⬜⬜⬜⬜⬜    | soily | 75    | 0, 75        |
