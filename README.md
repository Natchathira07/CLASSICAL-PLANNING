# ExpNo:10 Implementation of Classical Planning Algorithm
# Algorithm or Steps Involved:
<ol>
  <li>Define the initial state</li>
  <li>Define the goal state</li>
  <li>Define the actions</li>
  <li>Find a <b>plan</b> to reach the goal state</li>
  <li>Print the plan</li>
</ol>

## Program

```
from collections import deque


def find_plan(initial_state, goal_state, actions):

    def is_goal(state):
        return all(
            state.get(key) == value
            for key, value in goal_state.items()
        )

    def apply_action(state, action):
        new_state = state.copy()
        new_state.update(action['effect'])
        return new_state

    def state_key(state):
        return tuple(sorted(state.items()))

    queue = deque()
    queue.append((initial_state, []))

    visited = {state_key(initial_state)}

    while queue:

        current_state, plan = queue.popleft()

        # Check whether goal is reached
        if is_goal(current_state):
            return plan

        # Try all available actions
        for action_name, action in actions.items():

            # Check preconditions
            if all(
                current_state.get(key) == value
                for key, value in action['precondition'].items()
            ):

                # Apply action
                new_state = apply_action(
                    current_state,
                    action
                )

                key = state_key(new_state)

                if key not in visited:
                    visited.add(key)

                    new_plan = plan + [action_name]

                    queue.append(
                        (new_state, new_plan)
                    )

    return None


# Initial State
initial_state = {
    'A': 'Table',
    'B': 'Table'
}

# Goal State
goal_state = {
    'A': 'B',
    'B': 'Table'
}

# Actions
actions = {

    'move_A_to_B': {
        'precondition': {
            'A': 'Table',
            'B': 'Table'
        },
        'effect': {
            'A': 'B'
        }
    },

    'move_B_to_Table': {
        'precondition': {
            'A': 'Table',
            'B': 'B'
        },
        'effect': {
            'B': 'Table'
        }
    }
}


# Find the plan
plan = find_plan(
    initial_state,
    goal_state,
    actions
)

print(plan)
```

# Example - 1
```
initial_state = {'A': 'Table', 'B': 'Table'}
goal_state = {'A': 'B', 'B': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_Table': {'precondition': {'A': 'Table', 'B': 'B'}, 'effect': {'B': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B']
```
# Example - 2
```
initial_state = {'A': 'Table', 'B': 'Table', 'C': 'Table'}
goal_state = {'A': 'B', 'B': 'C', 'C': 'Table'}

actions = {
    'move_A_to_B': {'precondition': {'A': 'Table', 'B': 'Table'}, 'effect': {'A': 'B'}},
    'move_B_to_C': {'precondition': {'A': 'B', 'B': 'Table', 'C': 'Table'}, 'effect': {'B': 'C'}},
    'move_C_to_Table': {'precondition': {'A': 'B', 'B': 'C', 'C': 'C'}, 'effect': {'C': 'Table'}}
}

plan = find_plan(initial_state, goal_state, actions)
print(plan)
```
# Output:
```
['move_A_to_B', 'move_B_to_C']
```
## Output

<img width="628" height="125" alt="image" src="https://github.com/user-attachments/assets/760d9542-8b6b-4e22-b822-f466d9d0142b" />

## Result
Thus, the Classical Planning Algorithm was successfully implemented using Breadth-First Search (BFS), and a valid sequence of actions was obtained to reach the goal state.
