# How To Run Longest6 v2

This note gives a short, practical path for running Longest6 v2.

The benchmark is provided by [autonomousvision/carla_garage](https://github.com/autonomousvision/carla_garage). If you are using the helper scripts in this repo, they are under `newtest_carla/` and already target the official Longest6 v2 route split.

## What Longest6 v2 Is

- 36 long routes
- about 1-2 km per route
- Towns 01-06
- 7 scenario types
- metrics: DS, RC, IS

Use Longest6 v2 when you care about long-horizon robustness rather than only short-route scenario handling.

## Before You Start

You will need:

- CARLA 0.9.15
- a Python environment that can import CARLA, leaderboard, scenario_runner, and your model dependencies
- a model repo containing the evaluator and your agent code
- a checkpoint to evaluate

Useful references:

- [carla_garage](https://github.com/autonomousvision/carla_garage)
- [BLUE repository](https://github.com/George-Ling3/BLUE)

## Route Resources

Get the official route files from `carla_garage/leaderboard/data/`:

- `longest6.xml`
- `longest6_split/`

For most evaluation pipelines, `longest6_split/` is the more convenient choice because it stores one XML per route.

## Easiest Way To Run It

If your model is already integrated with the helper scripts in this repo, start from one of these wrappers:

- `newtest_carla/auto_eval_longest6_local1.sh`
- `newtest_carla/auto_eval_longest6_bsub2.sh`
- `newtest_carla/auto_eval_criticvla_longest6_local1.sh`
- `newtest_carla/auto_eval_criticvla_longest6_bsub2.sh`

Before running, edit the path and model settings in the script you choose. In particular, check:

- `REPO_PATH`
- `CARLA_ROOT`
- `MODEL_CHECKPOINT`
- `AGENT_FILE`
- `SEEDS`
- `GPU_ID` or cluster options
- `ROUTE_START` and `ROUTE_END`

Then run, for example:

```bash
bash newtest_carla/auto_eval_longest6_local1.sh
```

## Run The Orchestrator Directly

If you prefer a cleaner command, call the Python orchestrator directly:

```bash
python newtest_carla/start_auto_eval_newroutes_local.py \
  --repo_path /path/to/your/model_repo \
  --model_checkpoint /path/to/pytorch_model.pt \
  --benchmark longest6 \
  --route_path /path/to/carla_garage/leaderboard/data/longest6_split \
  --route_prefix longest6 \
  --carla_root /path/to/CARLA_0.9.15 \
  --agent_file /path/to/your/agent.py \
  --out_root /path/to/output_longest6 \
  --seeds 1 2 3 \
  --gpu_id 0 \
  --tries 3
```

The important arguments are:

- `--route_path`: the `longest6_split/` directory
- `--route_prefix`: `longest6`
- `--agent_file`: your agent entry file
- `--out_root`: where results, logs, and visualizations will be written
- `--seeds`: use more than one seed for reportable results

Useful optional arguments:

- `--route_start` and `--route_end` for debugging only part of the benchmark
- `--viz_scale` to reduce visualization storage
- `--only_test_failed True` to rerun selected failed routes

## How To Read The Results

After evaluation, aggregate the route JSON files with the official `carla_garage` parser:

```bash
python carla_garage/tools/result_parser.py \
  --xml carla_garage/leaderboard/data/longest6.xml \
  --results /path/to/output_longest6/<agent>/longest6/<seed>/res
```

This writes a `results.csv` file inside the results directory. The summary rows map to the main Longest6 v2 metrics:

- `Avg. driving score` → DS
- `Avg. route completion` → RC
- `Avg. infraction penalty` → IS

If you run multiple seeds, parse each seed separately and average DS, RC, and IS in your final report.

## Recommended Practice

- Run the full 36-route split for reportable numbers.
- Prefer 3 seeds when compute allows.
- Keep CARLA fixed at 0.9.15.
- Report clearly whether your result is single-seed or multi-seed.
- Keep route-level JSONs and logs so failures can be audited.

## Common Pitfalls

- Using the old Longest6 benchmark instead of Longest6 v2.
- Pointing to the wrong route directory instead of `longest6_split/`.
- Forgetting to check wrapper defaults such as route range.
- Running with an environment that cannot import CARLA, leaderboard, scenario_runner, and model code together.
- Reading IS alone without DS and RC.
