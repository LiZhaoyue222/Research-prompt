# Research Code Writing Plan

Keep each file under 300 lines and the total number of files at 10 or fewer; use as few files as possible. First, explain what you plan to do in no more than 300 words. Then write detailed criteria for subagents that correspond to those actions.

The PLAN must report the following:

## 1. File Structure

List the files, their names, and the content each contains. In general, the basic files include the following **9 files** (you may start here and merge files as the task requires; this is a concise arrangement based on project responsibilities, not a requirement to use all of these files):

| File | Contents |
| --- | --- |
| `data.py` | Data acquisition, validation, splitting, preprocessing, and loading; post-processing when needed |
| `model.py` | Model structure, initialization, forward computation, and concise, closely related losses |
| `train.py` | Optimizer, training loop, validation calls, logging, saving, and restoring |
| `infer.py` | Load the model, process new data, output predictions, and provide reusable prediction functions |
| `evaluate.py` | Reuse prediction logic, calculate metrics, and aggregate results |
| `test_core.py` | Necessary tests for core formulas, boundaries, training smoke tests, and save/load behavior |
| `config.toml` | Data, model, training, and evaluation parameters; include baseline and ablation configurations when needed |
| `pyproject.toml` | Python and dependency constraints, and test-tool configuration |
| `README.md` | Method sources, installation, commands, experiment reproduction, and interpretation of results |

## 2. Specifications

Define the research objective, training optimization objective, and evaluation metrics separately. Specify the parameters, return values, types, valid ranges, exceptions, and side effects of core interfaces; describe hard constraints, defaults, and configuration override order. Provide measurable acceptance criteria and distinguish runtime correctness, numerical correctness, and research-effectiveness goals.

## 3. Data

For key tensors, list the meaning of each axis, shape, dtype, device, and value range; specify the dimensions used for matrix multiplication, transposition, broadcasting, masks, and reduction. Define the data source and version, split unit, randomness, preprocessing fit scope, and consistency between training and inference. Specify behavior for empty input, a single element, all-zero input, duplicates, missing values, invalid shapes, and a final batch smaller than one full batch; explicitly rejecting invalid input is allowed.

## 4. Workflow

List the steps in order: initialization, data preparation, training, validation, model selection, testing, and inference. Distinguish epoch, batch, micro-step, and optimizer step; specify counting origins, loop intervals, and the actual number of updates. Describe the order of gradient zeroing, backpropagation, accumulation, clipping, optimizer, and scheduler operations, as well as validation frequency, stopping conditions, tie-breaking rules for the best model, and checkpoint-resumption semantics.

## 5. Mathematics

Write the total objective and each loss term as a formula, including optimization direction, weights, summation scope, denominator, and reduction. Use a correspondence table to explain symbols, dimensions, transposes, broadcasting, gradient paths, and detach locations, and map them to implementation functions. Separately specify where and how often data standardization, network normalization, loss reduction, and gradient scaling occur. For applicable cases involving division by zero, log(0), overflow, NaN/Inf, zero variance, and precision loss, list handling strategies and validation cases; do not add epsilon or clipping without explaining its mathematical effect.

## 6. Reproducibility and Evaluation

Specify seed and random-state management, the actual configuration, data-split identifiers, and how code and environment versions are recorded. Define checkpoint contents and restoration guarantees; distinguish “able to continue training” from “able to reproduce the same training trajectory.” Specify validation-set model selection, test-set usage, metric aggregation, baselines, required ablations, and repeated-experiment plans, with scale constrained by the compute budget.

## 7. Validation

Every key requirement must have a corresponding validation method and pass condition. As applicable, cover hand-calculated small samples or comparison with a reference implementation, shape and boundary tests, gradient checks, short training smoke tests, overfitting on a miniature dataset, prediction consistency after save/load, and training resumption. Provide installation, test, training, evaluation, and inference commands. Clearly distinguish checks that passed, were not run, or were blocked by the environment; never present planned checks as already verified.

## 8. Subagent Task Contract

Split tasks according to the actions above. For each task, specify its goal, prerequisites, files it may modify, interfaces and mathematical definitions it must follow, acceptance tests, and deliverables. Fix shared interfaces first, then work on independent tasks in parallel; assign one owner to each file. Subagents must not change formulas, data splits, or interfaces, or add files on their own; report conflicts to the main agent. The main agent is responsible for integration, end-to-end validation, and the final file-count and line-count checks.
