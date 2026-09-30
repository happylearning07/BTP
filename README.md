## Fixed candidate dataset

Dataset:

MATH500

For each of 500 questions, 8 candidate solutions were generated with:

Qwen2.5-7B-Instruct

Therefore:

500 questions × 8 candidates = 4000 candidates

The candidate pool is fixed for the current baseline experiments so that later verifier comparisons can use the same candidates.

## Candidate correctness

Observed benchmark results:

Total candidates:       4000
Benchmark-correct:      2760
Benchmark-incorrect:    1240

Candidate correctness rate:

2760 / 4000 = 69.0%

Important caveat:

correct=False comes from the benchmark answer-evaluation pipeline. The inspected evaluator can mark an answer incorrect when the predicted answer cannot be parsed.

Therefore, the 1240 benchmark-incorrect candidates should not yet be interpreted as 1240 mathematically wrong solutions.

A later diagnostic should split them into:

parser failures

successfully parsed but mathematically incorrect candidates

## Vanilla Qwen3-4B verifier

Before running the full agentic AgentV-RL verifier, a single-shot Qwen3-4B baseline was evaluated.

Input:

Original mathematical problem
+
Candidate solution

Output included:

judge = TRUE/FALSE
confidence score
verification explanation

The benchmark evaluator remained separate:

correct
    = benchmark/oracle correctness

judge
    = Qwen3-4B verifier decision

score
    = Qwen3-4B confidence

verification
    = verifier explanation

This separation will be preserved in later experiments.

<img width="205" height="283" alt="image" src="https://github.com/user-attachments/assets/a41f136f-bf2a-44c5-9bcb-847e649edd84" />


## Full vanilla evaluation

The experiment evaluated all:

4000 candidates

The first run encountered a CUDA OOM during vLLM wake-up/sleep-mode behavior after about 280 records.

The completed records were preserved and the run was resumed rather than restarted.

Final output:

data/processed/vanilla_qwen3_4000.jsonl

The final output contained 500 JSONL records, corresponding to all 500 MATH500 questions and their 8 candidates.

Approximate output size:

82 MB

## Verified vanilla results

Final metrics over all 4000 candidate evaluations:

Metric

Result

Accuracy

0.5725

Precision

0.8110

Recall

0.4960

F1

0.6156

Confusion matrix:

                 Predicted
               FALSE    TRUE

Actual FALSE     921     319
Actual TRUE     1391    1369

Therefore:

TN = 921
FP = 319
FN = 1391
TP = 1369

Observed behavior:

precision is relatively high when the verifier predicts TRUE

recall is substantially lower

there are many false negatives

Most importantly, this is a single-shot vanilla Qwen3-4B verifier, not the full AgentV-RL agentic verifier.

These numbers must therefore not be reported as AgentV-RL results.

## What has been completed

✓ AgentV-RL repository inspection
✓ Local environment setup
✓ Fixed MATH500 candidate pool
✓ 4000 candidate solutions
✓ Benchmark correctness evaluation
✓ Full vanilla Qwen3-4B evaluation
✓ Accuracy / precision / recall / F1
✓ Confusion matrix
✓ Reusable fixed candidate pool for later comparisons

This is the current baseline/foundation stage of the research.

## What remains

The actual proposed research contribution has not yet been trained.

Pending:

[ ] AgentV-RL reproduction/evaluation
[ ] AgentV-RL vs vanilla comparison
[ ] Strategic attack-only pilot
[ ] Strategic actor / verifier-aware candidate generation
[ ] SMV
[ ] CDAV
[ ] DCFR
[ ] Full CRAFT-V integration
[ ] Ablations
[ ] Transfer/generalization experiments
[ ] Efficiency analysis
[ ] Final statistical evaluation
