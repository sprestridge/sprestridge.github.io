---
layout: post
title: "Predicting the final four"
date: 2025-05-23 12:00:00 -0500
categories: [data science, python, lacrosse]
excerpt_separator: <!--more-->

---
### Who is going to win it all?

Heading into Memorial Day weekend the [Cornell Big Red](https://cornellbigred.com/sports/mens-lacrosse) have to feel pretty confident. They've won their first two playoff games over UAlbany by a score of 15-6, and Richmond by a score of 13-12, and have **increased their odds of winning the whole tournament from near 30% to 44%**.

How did I determine the odds of each team winning the tournament? Monte Carlo analysis, [power ratings](https://laxnumbers.com/ratings.php?y=2025&v=401), and [poisson distributions](https://en.wikipedia.org/wiki/Poisson_distribution) to the rescue. Read on to learn more.

<!--more-->

| Team         | Record | Rating | AGD  | GF  | GA  | Odds |
|:-------------|-------:|-------:|-----:|----:|----:|-----:|
| Cornell      | 15-1   | 99.99  | 5.82 | 275 | 176 | 45%  |
| Penn State   | 11-5   | 98.36  | 3.25 | 194 | 146 | 15%  |
| Maryland     | 13-3   | 98.44  | 3.18 | 177 | 127 | 18%  |
| Syracuse     | 12-5   | 98.51  | 3.72 | 250 | 183 | 23%  |

For this analysis, I am using the ratings to calculate the expected goal difference for two teams playing one another. Game scores for sports follow a Poisson distribution. If we know the expected goals for each team, we can sample from Poisson distributions to get a simulated score and determine a winner.

If you are having trouble figuring out what that means in practical terms, the [Posson Distribution Calculator](https://ezcalc.me/poisson-distribution-calculator/) has several good examples explaining usage. Also, a few lacrosse example may be instructive.

Use the mens average goals per game of 22.54 as the average rate (lambda) in the calculator and enter 25 for occurrences (k)–in this case, goals scored. With the type of Poisson probability set to _no more than "k" occurrences_, the calculator will output a probability (P) of 0.7406 which means that for 74.1% of the games played we can expect the total goals scored by both teams to be 25 goals or less.

The Maryland vs Georgetown game from last weekend is also instructive. The final score was 9-6 which means 15 total goals were scored. Using the calculator again tells us that we would expect just 6.2% of games to have 15 total goals or less. Yes, the game was slower paced than most and had far less scoring than most.

The power ratings in the table above likely differ slightly from those published by [LaxNumbers](https://laxnumbers.com/ratings.php?y=2025&v=401). I have calculated the ratings myself.

Using the data in the table above and the script included below, the following command in the terminal will return the tournament win probabilities for each of the four teams.

`python3.10 mc_final_four.py 10000 22.54 99.99 98.36 98.44 98.51`

```Markdown
Tournament Win Probabilities:
Team 1 - Cornell: 44.68%
Team 2 - Penn State: 14.69%
Team 3 - Maryland: 17.93%
Team 4 - Syracuse: 22.70%
```

Regarding the average total goals per game, I did an analysis for the mens and womens teams remaining to calculate the average across all games played. For men, it was **22.54 goals per game** and for women it was **24.69 goals per game**.

### Women's Final Four

Also, here are the odds for the Women's bracket. North Carolina (48%) and Boston College (47%) are heavily favored to win the weekend:

| Team           | Record | Rating | AGD  | GF  | GA  | Odds |
|:---------------|-------:|-------:|-----:|----:|----:|-----:|
| North Carolina | 20-0   | 99.79  |10.40 | 343 | 135 | 48%  |
| Florida        | 20-2   | 93.54  | 7.63 | 364 | 196 |  1%  |
| Boston College | 19-2   | 99.99  | 9.80 | 364 | 158 | 47%  |
| Northwestern   | 18-2   | 96.17  | 7.75 | 322 | 167 |  4%  |

`python3.10 mc_final_four.py 10000 24.68 99.79 93.54 99.99 96.17`

```Markdown
Tournament Win Probabilities:
Team  1: 46.98%
Team  2: 0.93%
Team  3: 47.33%
Team  4: 4.76%
```

---

### How to Run from the Command Line

1.  **Save the code:** Save the script below as a Python file, for example, `mc_final_four.py`.

2.  **Open your terminal or command prompt.**

3.  **Navigate to the directory** where you saved the file.

4.  **Run the script** using the `python` command, followed by the script name and the arguments:

```bash
python mc_final_four.py <num_simulations> <average_total_goals_per_game> <Team1_rating> <Team2_rating> <Team3_rating> <Team4_rating>
```

**Example using your provided ratings (with `average_total_goals_per_game` at 23):**

```bash
python mc_final_four.py 10000 23 99.79 93.54 99.99 96.17
```

Here's what each argument represents:

- `10000`: The number of tournament simulations to run.
- `23`: The `average_total_goals_per_game` for the sport.
- `99.79`: Team 1's power rating (North Carolina).
- `93.54`: Team 2's power rating (Florida).
- `99.99`: Team 3's power rating (Boston College).
- `96.17`: Team 4's power rating (Northwestern).

---

### Notes

This version is more convenient for repeated analysis with different parameters without editing the code directly.  

- **`argparse` Module:** This is Python's standard library for parsing command-line arguments.
- `parser = argparse.ArgumentParser(...)`: Creates an argument parser object.
- `parser.add_argument(...)`: Defines each expected argument (its name, type, and help text).
- `args = parser.parse_args()`: Parses the arguments provided on the command line.
- **Input Handling:**
- The `input()` prompts have been removed.
- The values for `num_simulations`, `average_total_goals_per_game`, and the four team ratings are now read directly from `args.<argument_name>`.
- **`average_total_goals_per_game` as a parameter:** This crucial value is no longer hardcoded within `simulate_game`. It's now passed down from the `main` function through `run_monte_carlo_analysis` and `simulate_tournament` to `simulate_game`, allowing you to easily adjust it from the command line.
- **Input Validation:** Basic checks are added to ensure `num_simulations` and `average_total_goals_per_game` are positive values.

### Script

```python
import random
import math
import argparse
from scipy.stats import poisson

def simulate_game(team1_power, team2_power, average_total_goals_per_game):
    """
    Simulates a single game between two teams based on their power ratings,
    modeling the difference in ratings as an expected goal difference,
    and then using Poisson distribution for scores.
    Returns True if team1 wins, False if team2 wins.
    """
    expected_goal_difference = team1_power - team2_power

    team1_expected_goals = (average_total_goals_per_game + expected_goal_difference) / 2
    team2_expected_goals = (average_total_goals_per_game - expected_goal_difference) / 2

    # Ensure expected goals are positive and reasonable for Poisson distribution.
    min_expected_goals = 0.1
    team1_expected_goals = max(min_expected_goals, team1_expected_goals)
    team2_expected_goals = max(min_expected_goals, team2_expected_goals)

    # Simulate scores using Poisson distribution
    team1_score = poisson.rvs(team1_expected_goals)
    team2_score = poisson.rvs(team2_expected_goals)

    # Determine the winner
    if team1_score > team2_score:
        return True  # Team 1 wins
    elif team2_score > team1_score:
        return False # Team 2 wins
    else:
        # Tie-breaker: If scores are tied, the team with the higher power rating wins.
        return team1_power > team2_power

def simulate_tournament(initial_round_teams_with_ids, average_total_goals_per_game):
    """
    Simulates a single 4-team tournament given the initial list of (team_id, power_rating) tuples
    for the first round matchups (1 vs 2, 3 vs 4).
    Returns the ID of the winning team.
    """
    # Round 1: Semifinals
    # Game 1: Team at index 0 vs Team at index 1
    team1_id, team1_power = initial_round_teams_with_ids[0]
    team2_id, team2_power = initial_round_teams_with_ids[1]
    
    if simulate_game(team1_power, team2_power, average_total_goals_per_game):
        winner1 = (team1_id, team1_power)
    else:
        winner1 = (team2_id, team2_power)

    # Game 2: Team at index 2 vs Team at index 3
    team3_id, team3_power = initial_round_teams_with_ids[2]
    team4_id, team4_power = initial_round_teams_with_ids[3]

    if simulate_game(team3_power, team4_power, average_total_goals_per_game):
        winner2 = (team3_id, team3_power)
    else:
        winner2 = (team4_id, team4_power)

    # Round 2: Championship
    finalist1_id, finalist1_power = winner1
    finalist2_id, finalist2_power = winner2

    if simulate_game(finalist1_power, finalist2_power, average_total_goals_per_game):
        return finalist1_id
    else:
        return finalist2_id

def run_monte_carlo_analysis(initial_teams_with_ids, num_simulations, average_total_goals_per_game):
    """
    Runs a Monte Carlo simulation to estimate each team's chance of winning.
    'initial_teams_with_ids' should be a list of (team_id, power_rating) tuples.
    """
    all_team_ids = sorted(list(set([team_id for team_id, _ in initial_teams_with_ids])))
    win_counts = {team_id: 0 for team_id in all_team_ids}

    print(f"Running {num_simulations} tournament simulations...")
    for _ in range(num_simulations):
        winner_id = simulate_tournament(initial_teams_with_ids, average_total_goals_per_game)
        win_counts[winner_id] += 1

    print("\nTournament Win Probabilities:")
    for team_id in sorted(win_counts.keys()):
        probability = (win_counts[team_id] / num_simulations) * 100
        print(f"Team {team_id:2d}: {probability:.2f}%")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Run a Monte Carlo simulation for a 4-team tournament.")
    parser.add_argument("num_simulations", type=int, help="Number of simulations to run.")
    parser.add_argument("average_total_goals_per_game", type=float, 
                        help="Average total goals scored in a game for the sport being simulated.")
    parser.add_argument("team1_rating", type=float, help="Power rating for Team 1.")
    parser.add_argument("team2_rating", type=float, help="Power rating for Team 2.")
    parser.add_argument("team3_rating", type=float, help="Power rating for Team 3.")
    parser.add_argument("team4_rating", type=float, help="Power rating for Team 4.")

    args = parser.parse_args()

    # Validate inputs
    if args.num_simulations <= 0:
        print("Error: Number of simulations must be positive.")
        exit(1)
    if args.average_total_goals_per_game <= 0:
        print("Error: Average total goals per game must be positive.")
        exit(1)

    power_ratings = [
        args.team1_rating,
        args.team2_rating,
        args.team3_rating,
        args.team4_rating
    ]

    initial_tournament_teams = []
    for i, power in enumerate(power_ratings):
        initial_tournament_teams.append((i + 1, power))

    run_monte_carlo_analysis(
        initial_tournament_teams,
        args.num_simulations,
        args.average_total_goals_per_game
    )

```
