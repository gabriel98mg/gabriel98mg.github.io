# Recruitment Analytics for CF Real Betis  
## Identifying Cost-Effective Rotation Striker Targets

## Project Overview

Real Betis finished 5th in La Liga during the 2024–25 season and secured qualification for the UEFA Champions League. With an increased fixture load next season (2025-26) and the potential departure of Cédric Bakambu, the club faces a need for additional attacking depth at centre-forward.

The objective is to identify a striker capable of serving as a rotation option behind Juan Camilo "Cucho" Hernández while also offering potential for future market value appreciation.

Given Real Betis' financial constraints, the club cannot consistently compete for established elite forwards. As a result, a data-driven recruitment process can help identify undervalued players before their market value rises.

Álvaro Ladrón de Guevara, Head Scout at Real Betis, has requested a shortlist of seven budget-conscious (≤ €15 mill) striker targets competing in Europe's top five leagues: the Premier League (UK), La Liga (Spain), Bundesliga(Germany), Serie A (Italy), and Ligue 1 (France).

This report applies statistical analysis and player profiling techniques to identify candidates who combine strong on-field performance, stylistic compatibility with Cucho Hernández, and realistic transfer feasibility.

## Data Structure

This project combines player-season performance data from FBref with market value, profile, injury, transfer, and national team data from Transfermarkt.

### Main Data Sources

```mermaid
erDiagram
    FBREF_PLAYER_SEASONS {
        string player
        int season
        string league
        string nation
        string pos
        numeric age
        numeric playing_time_min
        numeric playing_time_90s
        numeric playing_time_starts
        numeric expected_x_g
        numeric expected_x_a
        numeric per_90_minutes_npx_g
        numeric per_90_minutes_x_ag
        numeric sca_sca90
        numeric gca_gca90
        numeric expected_g_x_g
        numeric standard_sh_90
        numeric progression_prg_c
        numeric total_cmp_pct
        numeric prg_p
        numeric take_ons_succ_pct
    }

    TRANSFERMARKT_PLAYERS {
        int player_id
        string name
        string position
        string sub_position
        numeric market_value_in_eur
    }

    TRANSFERMARKT_PLAYER_VALUATIONS {
        int player_id
        date date
        numeric market_value_in_eur
    }

    PLAYER_PROFILES {
        int player_id
        string player_name
        date date_of_birth
        string citizenship
        string position
        string foot
        int current_club_id
        string current_club_url
        date joined_on
        date contract_expires
        string player_main_position
        string player_sub_position
    }

    PLAYER_INJURY_HISTORIES {
        int player_id
        string season
        string injury_reason
        date from_date
        date end_date
        numeric days_missed
        numeric games_missed
    }

    PLAYER_NATIONAL_TEAM_PERFORMANCES {
        int player_id
        int team_id
        string team_name
        date first_game_date
        numeric matches
        numeric goals
    }

    PLAYER_PERFORMANCES {
        int player_id
        string season
        string competition_id
        int team_id
        string competition_name
        string team_name
        numeric goals
        numeric assists
        numeric minutes_played
        numeric yellow_cards
        numeric red_cards
    }

    COMPETITIONS {
        string competition_id
        string competition_slug
        string competition_name
    }

    TEAMS_DETAILS {
        int club_id
        string club_name
        string country_name
        int season_id
        string competition_id
        string competition_name
        string club_division
    }

    FBREF_PLAYER_SEASONS ||--o| TRANSFERMARKT_PLAYERS : "matched by cleaned player name"
    TRANSFERMARKT_PLAYERS ||--o{ TRANSFERMARKT_PLAYER_VALUATIONS : "has annual market values"
    TRANSFERMARKT_PLAYERS ||--o| PLAYER_PROFILES : "matched by player_id"
    PLAYER_PROFILES ||--o{ PLAYER_INJURY_HISTORIES : "has injury history"
    PLAYER_PROFILES ||--o{ PLAYER_NATIONAL_TEAM_PERFORMANCES : "has national team record"
    PLAYER_PROFILES ||--o{ PLAYER_PERFORMANCES : "has club performance record"
    PLAYER_PERFORMANCES }o--|| COMPETITIONS : "played in competition"
    PLAYER_PERFORMANCES }o--|| TEAMS_DETAILS : "played for team"
    PLAYER_NATIONAL_TEAM_PERFORMANCES }o--|| TEAMS_DETAILS : "national team"
```

## Methodology

The analysis follows four main steps:

1. **Candidate screening**  
   Filtered players by position, age, minutes played, and realistic market value.

2. **Statistical profiling**  
   Compared players using attacking, creative, pressing, and possession-related metrics.

3. **Cucho-style comparison**  
   Evaluated which players most closely resemble Cucho Hernández's statistical and tactical profile.

4. **Recruitment ranking**  
   Combined performance indicators with market value to identify cost-effective targets.

## Key Findings

Ignoring the Betis Priority Score and Cucho Fit Score, the players who most resemble Cucho Hernández stylistically are:

| Rank | Player | Why He Fits |
|---:|---|---|
| 1 | Santiago Castro | Mobile striker, active presser, involved outside the box |
| 2 | Christantus Uche | Strong physical profile, high work rate, disruptive movement |
| 3 | Nick Woltemade | Excellent link-up play and ball progression |
| 4 | Lucas Stassin | Strong attacking profile but more goal-focused |
| 5 | Breel Embolo | Physical, experienced, useful stylistic overlap |

## Top Recommendation

**Santiago Castro** appears to be the closest stylistic comparison to Cucho Hernández.

He is not just a penalty-box striker. His value comes from movement, pressing, link-up play, and involvement in attacking sequences outside the box.

## Tools Used

- R
- tidyverse
- ggplot2
- Quarto
- GitHub

## Repository Structure

```text
betis-striker-recruitment/
│
├── README.md
├── report/
│   └── betis_recruitment_report.pdf
│
├── images/
│   ├── workflow.png
│   ├── radar_castro.png
│   ├── radar_uche.png
│   └── radar_woltemade.png
│
├── scripts/
│   ├── data_cleaning.R
│   ├── similarity_model.R
│   └── visualizations.R
│
└── data/
