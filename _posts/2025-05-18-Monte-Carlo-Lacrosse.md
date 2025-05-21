---
layout: post
title: "Predicting outcomes of lacrosse games"
date: 2025-05-18 12:00:00 -0500
categories: [data science, python, lacrosse]
excerpt_separator: <!--more-->

---

This weekend is the NCAA Division I Mens Lacrosse Quarterfinals. I wrote a Python script to perform **Monte Carlo analysis on game outcomes for two teams** based on the **ratings** and **average goal differential (AGD)** provided by [LaxNumbers](https://www.laxnumbers.com?ref=sprestridge.net). I'm not sold on the approach taken in the script but it takes into account the two teams relative ratings and their ability to score more goals than their opponents (via the AGD).

Predicted winners are Cornell, Syracuse, Notre Dame, and Maryland.

Click through for details.

<!--more-->

Monte Carlo simulation is a statistical technique to approximate the outcome of an event by generating random samples from a probability distribution. Here, I am using Monte Carlo simulation to predict the number of wins for each team in a series of matches (games).

Here are the quarterfinal teams sorted by rating, high to low. AGD is average goal differential, GF is goals for, and GA is goals against:

| Team         | Record | Rating | AGD  | GF  | GA  |
|:-------------|-------:|-------:|-----:|----:|----:|
| Cornell      | 15-1   | 99.99  | 6.12 | 275 | 176 |
| Notre Dame   | 9-4    | 99.52  | 5.00 | 179 | 114 |
| Maryland     | 13-3   | 98.43  | 3.12 | 177 | 127 |
| Syracuse     | 12-5   | 98.42  | 3.88 | 250 | 183 |
| Penn State   | 11-5   | 97.96  | 3.00 | 194 | 146 |
| Richmond     | 14-3   | 97.27  | 5.88 | 246 | 147 |
| Princeton    | 13-3   | 97.90  | 3.06 | 234 | 186 |
| Georgetown   | 11-5   | 95.20  | 3.12 | 196 | 146 |


**Sunday**

- Notre Dame, 9-4, rating 99.52, AGD 5.00, GF 179, GA 114

- Penn State, 11-5, rating 97.96, AGD 3.0, GF 194, GA 146

- Maryland, 13-3, rating 98.43, AGD 3.12, GF 177, GA 127

- Georgetown, 11-5, rating 95.20, AGD 3.12, GF 196, GA 146

I have several versions of Python installed on my system so in the examples below I am specifying use of Python3.10.

For **ND vs Penn State**:

`python3.10 mc_teams.py 5000 99.52 5.0 97.96 3.0`

**ND wins** 2,882 games or _57.64% of the time_.

For **MD vs Gtown**:

`python3.10 mc_teams.py 5000 98.43 3.12 95.20 3.12`

**Maryland wins** 2,817 games or _56.34% of the time_.

**Saturday**

Cornell and Syracuse each won yesterday by a single goal but let's look at the numbers.

- Cornell, 15-1, rating 99.99, AGD 6.12, GF 275, GA 176

- Richmond, 14-3, rating 97.27, AGD 5.88, GF 246, GA 147

- Syracuse, 12-5, rating 98.42, AGD 3.88, GF 250, GA 183

- Princeton, 13-3, rating 97.90, AGD 3.06, GF 234, GA 186

For **Cornell vs Richmond**:

`python3.10 mc_teams.py 5000 99.99 6.12 97.27 5.88`

**Cornell wins** 2,926 games or _58.52% of the time_.

Let's try entering the GF per game instead of the ratings for each team. So **Cornell** has 275 / 16 = **17.188 goals for per game** and **Richmond** has 246 / 17 = **14.471 goals for per game**:

`python3.10 mc_teams.py 5000 17.188 3.88 14.471 3.06`

In this case, Cornell wins 3,494 games or _69.88% of the time_. This method significantly favors Cornell and I do not think is as good as the other.

For **Syracuse vs Princeton**:

`python3.10 mc_teams.py 5000 98.42 3.88 97.90 3.06`

**Syracuse wins** 2,640 games or _52.80% of the time_.

Python Script, `mc_teams.py` is shown below. I'm not sold on the rating plus AGD to calculate a team's "expected goals", it's more a total expected rating, but the approach of using the Poisson distribution is sound.

### Code Explanation

The code consists of two main functions: `simulate_match` and `monte_carlo_simulation`.

- The `simulate_match` function takes the ratings and AGD of the two teams as input and returns the number of goals scored by each team. It uses the following formulas to calculate the expected number of goals:

    - `team1_expected_goals = team1_rating + team1_agd`
    - `team2_expected_goals = team2_rating + team2_agd`

- The `monte_carlo_simulation` function takes the number of trials and the ratings and AGD of the two teams as input and returns the number of wins for each team. It uses a loop to repeat the simulation for the specified number of trials and keeps track of the number of wins for each team.

- The `main` function reads the command line arguments, calls the `monte_carlo_simulation` function, and prints the results.

### Example Use Case

To use this code, you would need to run it from the command line and provide the following arguments:

- `num_trials`: The number of trials to run the simulation for.
- `team1_rating`: The rating of the first team.
- `team1_agd`: The average goal difference of the first team.
- `team2_rating`: The rating of the second team.
- `team2_agd`: The average goal difference of the second team.

```python
#mc_teams.py
import numpy as np
import sys

def monte_carlo_simulation(num_trials, team1_rating, team1_agd, team2_rating, team2_agd):
    def simulate_match(team1_rating, team1_agd, team2_rating, team2_agd):
        team1_expected_goals = team1_rating + team1_agd
        team2_expected_goals = team2_rating + team2_agd
        
        team1_goals = np.random.poisson(team1_expected_goals)
        team2_goals = np.random.poisson(team2_expected_goals)
        
        return team1_goals, team2_goals

    team1_wins = 0
    team2_wins = 0
    
    for _ in range(num_trials):
        team1_goals, team2_goals = simulate_match(team1_rating, team1_agd, team2_rating, team2_agd)
        
        if team1_goals > team2_goals:
            team1_wins += 1
        elif team2_goals > team1_goals:
            team2_wins += 1
        else:
            if np.random.rand() < 0.5:
                team1_wins += 1
            else:
                team2_wins += 1
            
    return team1_wins,ref team2_wins

if __name__ == "__main__":
    # Read command line arguments
    if len(sys.argv) != 6:
        print("Usage: mc_teams.py <num_trials> <team1_rating> <team1_agd> <team2_rating> <team2_agd>")
        sys.exit(1)

    num_trials = int(sys.argv[1])
    team1_rating = float(sys.argv[2])
    team1_agd = float(sys.argv[3])
    team2_rating = float(sys.argv[4])
    team2_agd = float(sys.argv[5])

    team1_wins, team2_wins = monte_carlo_simulation(num_trials, team1_rating, team1_agd, team2_rating, team2_agd)

    print(f"After {num_trials} trials:")
    print(f"Team 1 Wins, %: {team1_wins}, {team1_wins / num_trials}")
    print(f"Team 2 Wins %: {team2_wins}, {team2_wins / num_trials}")
```
