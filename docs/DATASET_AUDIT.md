# CivicPulse dataset audit

Audit date: 2026-11-03  
Scope: files supplied in `datasets/`; no source-original file was modified.

## Verification summary

The workspace initially contained the dataset package only. The four supplied
inventory documents were read and their counts were checked against the
canonical files. The reported counts agree with the files inspected.

| Dataset | Verified records | Schema / labels | Missing values | Exact duplicate excess | Intended use |
| --- | ---: | --- | --- | ---: | --- |
| Civic complaints | 120 | `complaint_id`, `text`, `source_transcript`, `category`, `language`, `script`, source fields; 6 categories | 120 blank `is_synthetic` values (unknown provenance); 0 elsewhere | 0 | Small bilingual civic category benchmark |
| Hindi sentiment train | 4,182 | `sentiment`, `text`, `split`, `data_source`; negative/neutral/positive | 0 | 4 | Training only |
| Hindi sentiment validation | 523 | Same schema | 0 | 0 | Validation only |
| Hindi sentiment test | 523 | Same schema | 0 | 0 | Held-out evaluation only |
| Hinglish sentiment | 2,766 | `source_row_id`, `text`, `sentiment`, `label`, `data_source` | 0 | 0 exact full rows (46 repeated text values) | Supplementary Hinglish sentiment |
| Hindi NER train | 75,827 | JSON `id`, `tokens`, `ner_tags` | No token/tag length mismatches | n/a | NER training |
| Hindi NER validation | 10,851 | Same representation | No token/tag length mismatches | n/a | NER validation |
| Hindi NER test | 21,657 | Same representation | No token/tag length mismatches | n/a | Held-out NER evaluation |
| English–Hindi parallel dev | 520 aligned pairs | one sentence per line in `.en` and `.hi` | 0 blank lines | n/a | Inspection/development only |
| English–Hindi parallel test | 2,507 aligned pairs | one sentence per line in `.en` and `.hi` | 0 blank lines | n/a | Held-out evaluation only |

## Distributions verified

### Civic complaint classification

Each of the six categories has 20 rows: `electricity`, `healthcare`,
`ration_aadhar`, `roads_transport`, `sanitation`, and `water_supply`.
Language and script are balanced: 60 English/Latin and 60 Hindi/Devanagari.
All 120 IDs and analysis texts are unique. The original `metadata.csv` has
the same 120 transcripts and the twelve bilingual category-example files each
contain ten lines.

This is a very small benchmark. It must not be represented as production-grade
training data. Its supplied metadata references audio paths, but no audio files
are present. The archive establishes neither whether the transcripts are
authentic nor whether they are synthetic; `is_synthetic` is deliberately blank.

### Hindi sentiment

| Split | Negative | Neutral | Positive |
| --- | ---: | ---: | ---: |
| Train | 567 | 1,789 | 1,826 |
| Validation | 77 | 223 | 223 |
| Test | 68 | 214 | 241 |

The source material is Hindi in Devanagari and examples are general-domain
(reviews/news-like text), not civic-complaint ground truth. The supplied splits
are retained; no random re-split is permitted. Four duplicate train records
should be considered when reporting experiments and should not cross a future
derived split boundary.

### Hinglish sentiment

The supplied mapping is consistent: `0 → negative` (545), `1 → neutral`
(1,234), and `2 → positive` (987). Text is predominantly Roman-script,
mixed/general-domain content; it is not evidence of civic sentiment. Text has
2,720 distinct values, so 46 values repeat even though all full rows remain
unique because of the source-row identifier.

### Hindi NER

`labels.json` maps 23 numeric IDs to BIO labels: B/I variants of FESTIVAL,
GAME, LANGUAGE, LITERATURE, LOCATION, MISC, NUMEX, ORGANIZATION, PERSON,
RELIGION and TIMEX, plus `O` (ID 22). JSON and CoNLL are alternate
representations of the same splits and must never be concatenated. Every
inspected JSON record has equal token and tag lengths.

### Translation

Both `.en`/`.hi` files have matching line counts and no blank lines. There is
no supplied training split. Development pairs are not silently treated as
training data, and the test pairs remain held out. Any trainable Seq2Seq
demonstration must use a separately documented educational or permitted corpus.

## File formats and encoding observations

The canonical tabular data load successfully as CSV and the NER JSON parses as
JSON. Hindi text renders as UTF-8/Devanagari; English and Hinglish use Latin
script. NER CoNLL is available as an alternate source format but was not
counted independently. No automatic conversion of the immutable
`raw/source_originals/` archive is needed.

## Task mapping and known gaps

| Research task | Dataset status | Safe use |
| --- | --- | --- |
| Civic category classification | Available, small | Baseline with explicit small-data caveat |
| Hindi sentiment | Available with fixed splits | Independent experiment |
| Hinglish sentiment | Available, noisy/mixed | Independent supplementary experiment |
| Hindi NER | Available with fixed splits | CPU-friendly subset/experiment and inference |
| English–Hindi translation | Dev/test only | Alignment/evaluation; not model training |
| Civic urgency | Not available | Separate labelled synthetic/manual data only |
| Civic summarisation | No references | Local extractive baseline; no unsupported ROUGE claim |
| English/Hinglish civic sentiment | Not available | Do not claim gold-standard evaluation |

## Provenance, licence and safety limitations

The archive names `Akshay_2.zip`, but it provides no definitive publisher URLs,
licences, collection method, or redistribution terms. These details remain
**unknown and require verification** before release. Raw originals are
read-only inputs. Any imports, annotations, augmentation, generated records,
and experiment splits must be documented separately, labelled by provenance,
and kept out of public benchmark test data.
