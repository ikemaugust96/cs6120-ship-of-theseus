# Style vs. Content Decay in Iterative AI Paraphrasing

CS 6120 group project examining how repeated AI paraphrasing changes the linguistic style, semantic content, and source identity of a text.

## Research Question

Does repeated AI paraphrasing cause stylistic characteristics to decay faster than semantic content?

## Dataset

We use the [Ship of Theseus Paraphrased Corpus](https://github.com/tripto03/Ship_of_theseus_paraphrased_copus), which contains:

- Seven text domains
- Human and six LLM source categories
- Four paraphrasers: ChatGPT, PaLM, Pegasus, and DIPPER
- Original texts and up to three successive paraphrasing generations
- 19,527 training records
- 420,012 paraphrased records

The dataset repository has been successfully accessed, downloaded, and inspected. Raw dataset files are not stored in this repository.

## Planned Methods

- Lexical similarity: N-gram overlap, BLEU, and ROUGE
- Syntactic and stylistic analysis: POS distributions, sentence length, and dependency depth
- Semantic similarity: SBERT and BERTScore
- Source-attribution analysis across paraphrasing generations

## Repository Structure

- `data/`: Local data organization and metadata
- `notebooks/`: Exploratory analysis and experiments
- `src/`: Reusable data-processing and modeling code
- `experiments/`: Experimental outputs
- `figures/`: Generated visualizations
- `paper/`: Final paper materials
- `docs/`: Proposal and progress documentation

## Team Members

- Wenxuan Yang
- Min Liu
- Hang Liu