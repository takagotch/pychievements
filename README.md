### pychievements
---
https://github.com/PacketPerception/pychievements

```py
classMyAchievement(Archievement):
  name = "My Achievement"
  category = "achievements"
  keywords = ("my", "achievement")
  goals = (
    {"level": 10, "name": "Level 1", "icon": icons.star, "description": "Level One"},
    {"level": 20, "name": "Level 2", "icon": icons.star, "description": "Level Two"},
    {"level": 30, "name": "Level 3", "icon": icons.star, "description": "Level Three"}
  )

tracker.increment(user_id, MyAcievement)

tracker.evaluate(user_id, MyAchievement, some, extra, args)

tracker.achievements()
tracker.achieved(uid, achievemnet)
tracker.unachieved(uid, achievement)
tracker.current(uid, achievement)
```

```
pip install pychivements
```

```py
# exs/cli.py
import sys
import cmd
from pychievements import tracker, Achievement, icons
from pychievements.signals import receiver, goal_acieved, highest_level_achieved
from pychievements.cli import print_goal, print_goals_for_tracked

class TheLister(Achievement):












```


