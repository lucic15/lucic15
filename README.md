To fix the issue where running one YAML file removes content added by the other, you need to carefully manage how each GitHub Actions workflow updates your README file sections. Here's a structured approach to address this problem:

### Step-by-Step Solution:

1. **Identify Sections and Markers:**
   - In your README file, identify the sections that each GitHub Actions workflow is supposed to update. For example, you have `<!--START_SECTION:waka-simple-->` and `<!--START_SECTION:waka-->` markers.

2. **Separate Actions for Different Sections:**
   - Ensure that each GitHub Actions workflow (`waka-readme` and `waka-stats`) updates separate sections of the README file. Based on your YAML configurations, `waka-readme` updates `waka-simple` and `waka-stats` updates a section for more detailed WakaTime stats.

3. **Adjust YAML Configurations:**
   - Modify the YAML configurations (`waka-readme.yml` and `waka-stats.yml`) to update their respective sections without overlapping or removing each other's content.

### Example Modifications:

**waka-readme.yml:**
```yaml
name: Waka Readme

on:
  # Manual workflow trigger
  workflow_dispatch:
  # Scheduled daily update
  schedule:
    - cron: "0 0 * * *"

jobs:
  update-readme:
    name: Update Readme with Waka Simple Stats
    runs-on: ubuntu-latest
    steps:
      - name: Update Waka Simple Stats
        uses: athul/waka-readme@master
        with:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
          SECTION_NAME: "waka-simple"
          COMMIT_MESSAGE: "Updated simple Waka stats section"
          BLOCKS: "⣀⣄⣤⣦⣶⣷⣿"
          TIME_RANGE: all_time
          SHOW_TIME: true
          SHOW_MASKED_TIME: true
          SHOW_TITLE: true
```

**waka-stats.yml:**
```yaml
name: Waka Stats

on:
  # Manual workflow trigger
  workflow_dispatch:
  # Scheduled daily update
  schedule:
    - cron: "0 12 * * *"

jobs:
  update-readme:
    name: Update Readme with Waka Stats
    runs-on: ubuntu-latest
    steps:
      - name: Update Waka Stats
        uses: anmol098/waka-readme-stats@master
        with:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
          COMMIT_BY_ME: "False"
          COMMIT_MESSAGE: "Updated detailed Waka stats"
          SHOW_OS: "False"
          SHOW_PROJECTS: "True"
          SHOW_UPDATED_DATE: "True"
          SHOW_PROFILE_VIEWS: "False"
          SHOW_EDITORS: "False"
          SHOW_LANGUAGE: "True"
          SHOW_LANGUAGE_PER_REPO: "False"
          SHOW_LINES_OF_CODE: "True"
          SHOW_COMMIT: "True"
          SHOW_LOC_CHART: "False"
          SHOW_DAYS_OF_WEEK: "False"
          SHOW_SHORT_INFO: "True"
```

### README File:

```markdown
![64dbfb279110e5730b698a752532605b](https://github.com/lucic15/lucic15/assets/69390868/e4afab44-0bf1-4690-88ea-dc6e2ac6073f)

***

<p align="center">
  <img align="center" src="https://github-profile-trophy.vercel.app/?username=lucic15&theme=onedark&row=1&column=3" />
</p>

```yaml
name: Luka Lucic
from: Podgorica, Montenegro
job: null
fields_of_interests:
  - "Artificial Intelligence"
  - "Machine Learning"
  - "Cyber Security"
  - "Network Security"
  - "Internet of Things"
currently_learning:
  - "."
  - "."
  - "."
will_learn:
  - "."
hobbies:
  - "Gaming"
  - "Music"
  - "Learning"
```

***

<!--START_SECTION:waka-simple-->

<!--END_SECTION:waka-simple-->

<!--START_SECTION:waka-->

<!--END_SECTION:waka-->
```

### Key Points to Ensure:

- **Markers**: Ensure that `waka-readme.yml` updates `<!--START_SECTION:waka-simple-->` and `<!--END_SECTION:waka-simple-->`, while `waka-stats.yml` updates `<!--START_SECTION:waka-->` and `<!--END_SECTION:waka-->`.
  
- **No Overlap**: Each workflow should target its designated section without affecting the other. This prevents content from being overwritten or cleared unintentionally.

By following this approach, you should be able to update your README file with both simple and detailed WakaTime stats without one YAML file removing the content added by the other. Adjust the configurations as per your specific needs and ensure proper testing after modifications.
