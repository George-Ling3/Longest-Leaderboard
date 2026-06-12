# Longest6 v2 Leaderboard

Longest6 v2 is a long-horizon closed-loop autonomous driving benchmark that runs on the CARLA simulator. It contains 36 routes of about 1-2 km in Towns 01-06 with 7 scenario types, and is meant to test whether a driving model can remain robust over longer driving horizons.

This page collects public Longest6 v2 results that can be traced to papers or official project releases. Results are sorted by Driving Score (DS).

Longest6 v2 should be treated as a separate benchmark from the old leaderboard 1.0 Longest6 setting. The routes are adapted to CARLA 0.9.15 and the newer leaderboard and scenario-runner stack.

## Results

| Method | DS ↑ | RC ↑ | IS ↑ | Year | Venue |
| --- | ---: | ---: | ---: | ---: | --- |
| [TFv6](https://arxiv.org/abs/2512.20563) | 62 | 91 | 0.68 | 2026 | CVPR |
| [BLUE](https://arxiv.org/abs/2606.08684) | 36 | 84 | 0.43 | 2026 | - |
| [CriticVLA](https://arxiv.org/abs/2604.27366) | 34 | 66 | 0.55 | 2026 | - |
| [TF++](https://arxiv.org/abs/2412.09602) | 23 | 71 | - | 2024 | - |
| [SimLingo](https://arxiv.org/abs/2503.09594) | 22 | 70 | 0.38 | 2025 | CVPR |
| [HiP-AD](https://arxiv.org/abs/2503.08612) | 7 | 56 | - | 2025 | ICCV |

## Metrics

- `DS` is Driving Score, the main ranking metric.
- `RC` is Route Completion.
- `IS` is Infraction Score.

On long routes, DS is especially sensitive to compounded infractions. RC shows how far a method usually gets before termination. IS isolates the penalty term, so it is best read together with DS and RC.

## Resources

- Official benchmark description and route files: [autonomousvision/carla_garage](https://github.com/autonomousvision/carla_garage)
- Running guide: [LONGEST6_V2_TUTORIAL.md](LONGEST6_V2_TUTORIAL.md)

## Add A Result

If you want to add a method, please include:

- a public paper, project page, or official repository
- Longest6 v2 numbers with clear metric names
- whether the result is single-seed or multi-seed
- logs or a reproducible command when available
