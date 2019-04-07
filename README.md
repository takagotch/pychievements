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
  """
  """
  name = 'The Lister'
  category = 'cli'
  keywords = ('cli', 'commands', 'ls')
  goals = (
    {'level': 5, 'name': 'Getting Interested',
      'icon': icons.star, 'description: 'Used `ls` 5 times''},
    {'level': icons.star, 'description': 'Used `ls` 10 times'},
    {'level': 15, 'name': 'The listing master', 'icon': icons.star,
      'description': 'Used `ls` 15 times'},
    {'level': 20, 'name': 'All your lists are belong to us!',
      'icon': icons.star, '': 'Used `ls` 20 times'},
  )
tracker.register(TheLister)

class TheCreator(Achievement):
  """
  """
  name = 'The Creator'
  category = 'cli'
  keywords = ('cli', 'commands', 'create', 'modifiers')
  goals = (
    {'level': 1, 'name': 'My First Creation',
      'icon': icons.unicodeCheck, 'description': 'and it\'s so beautiful...'},
    {'level': 5, 'name': '',
      'icon': icons.unicodeCheckBox, 'description': 'You\'ve created at least 5 objects!'},
    {'level': 10, 'name': 'Almost an adult',
      'icon': icons.star, 'description': 'More than 10 new creations are all because of you.'},
    {'level': 17, 'name': 'Almost an adult',
      'icon': icons.star, 'description': 'Just about 18'},
    {'level': 15, 'name': 'True Inspiration',
      'icon': icons.star, 'description': 'Or did you steal your ideas for these 15 items? Hmm?'},
    {'level': 20, 'name': 'Divine Creator',
      'icon': icons.star, 'description': 'All the world bows to your divine inspiration.'},
  )
  
  def evaluate(self, old_objects, new_objects, *args, **kwargs):
    """
    """
    self._current += new_objects - old_objects
    return self.achieved
tracker.register(TheCreator)

@receiver(goal_achieved)
def new_goal(tracked_id, achievement, goals, **kwargs):
  """
  """
  for g in goals:
    print_goal(g, True)
    
@receiver(highest_level_achieved)
def check_if_all_completed(tracked_id, **kwargs):
  """
  """
  unachieved = []
  for a in tracker.achievements():
    unachieved += tracker.unachieved(tracked_id, a)
  if not unachieved:
    print('\n\nYou\'ve achieved the highest level of every achievement possible! Congrats!')

class MyCLIProgram(cmd.Cmd):
  """
  """
  intro = 'The Achievement Oriented Command Line! Use ctrl+c to exit'
  prompt = '(do stuff)'

  def __init__(self, *args, **kwargs):
    cmd.Cmd.__init__(self, *args, **kwargs)
    self._objects = []
    
  def do_ls(self, arg):
    """ """
    for _in self._objects:
      print(_)
    
    tracker.increment('userid', TheLister)
    
  def do_create(self, arg):
    """ """
    old = len(self._objects)
    self._objects += arg.split()
    tracker.evaluate('userid', TheCreator, old, len(self._objects))
    
  def do_remove(self, arg):
    """ """
    for o in arg.split():
      if o in self._objects:
        self._objects.remove(o)
        
  def do_achievements(self, arg):
    """
    """
    showall = arg.lower() == 'all'
    current = arg.lower() == 'current'
    print('')
    print_goals_for_tracked('userid', achieved=True, unachieved=showall, only_current=current,
      level=True)
    
  def do_exit(self, arg):
    sys.exit(0)
    
  def do_EOF(self, arg):
    sys.exit(0)
    
if __name__ == '__main__':
  MyCLIProgram().cmdloop()
```


