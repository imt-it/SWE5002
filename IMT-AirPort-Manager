# University of Bolton - Greater Manchester
# SWE5002 - Data Structures and Algorithms
# Tutor: Stylianos Angelopoulos
# Assessment 1
# Student: Ioannis Marios Tsiakoulias 
# Program Name: IMT-Airport-Manager
# -
# [Assessment - Brief]:
# -
# A small airport, with low traffic, needs to maintain landing and takeoffs. To do so, it maintains
# two queues where planes requesting to land or to takeoff are added, respectively.
# Planes should not wait long time on air until landing, so a takeoff will be allowed only if the
# landing queue is empty.
# It is possible that a plane requesting landing may have a problem (malfunction, low level of
# fuel, etc). In this case the landing will be given highest priority and allowed to land before any
# other landing request in the queue.
#   a). Define the appropriate data structures to implement this application
#   b). Implement a simulation that will randomly add requests (landing, takeoff, emergency
#       landing) and give allowance to the proper action.
# -
# [Example output of the simulation]:
# -
# | --------------------------------------- |
# | Flight 345 requests landing             |
# | Flight 190 requests landing             |
# | Flight 188 requests takeoff             |
# | CONTROL: 345 land                       |
# | Flight 621 requests emergency landing   |
# | CONTROL: 621 land                       |
# | Flight 511 requests takeoff             |
# | CONTROL: 190 land                       |
# | CONTROL: 188 takeoff                    |
# | CONTROL: 511 takeoff                    |
# | Flight 810 requests takeoff             |
# | CONTROL: 810 takeoff                    |
# | --------------------------------------- |

# --- Python Library Imports
import random # Used for random.randint(), random.choice() and random.uniform() functions
import time # Used for time.time() and time.sleep() functions

# --- Custom Print Function
def FN_Print(message, category):
    if category == 0: # Standard Message
        print(message)
    elif category == 1 and show_debug: # Debug Message (if enabled)
        print(message)

# --- Plane Class 
class CL_Plane:
    # Plane Object Initialization
    def __init__(self, flight_number, requested_action='none', condition='none'):
        self.flight_number = flight_number # Flight number
        self.requested_action = requested_action # 'landing' or 'takeoff'
        self.condition = condition # 'normal' or 'emergency'
        self.status = 'none' # 'none', 'wait', 'proceed', 'success'
        self.request_time = time.time() # Track when the plane entered the queue
        self.wait_duration = 0 # Time spent waiting in queue

# --- Queue Class
class CL_Queue:
    # Queue Object Initialization
    def __init__(self):
        # Two internal lists act as the landing and takeoff queues
        self.landing_queue = []
        self.takeoff_queue = []
    # Appends or inserts a plane to the appropriate queue based their request and condition
    def enqueue(self, plane):
        plane.status = 'wait'
        # Takeoff planes
        if plane.requested_action == 'takeoff':
            self.takeoff_queue.append(plane) # Can be appended (respecting queue FIFO)
            FN_Print(f"[DEBUG]: Flight {plane.flight_number} appended in takeoff queue. Takeoff queue length: {len(self.takeoff_queue)}.", 1)
        # Landing planes
        elif plane.requested_action == 'landing':
            # Have normal or emergency conditions
            if plane.condition == 'normal':
                self.landing_queue.append(plane) # Normal planes can be appended (respecting queue FIFO)
                FN_Print(f"[DEBUG]: Flight {plane.flight_number} appended in landing queue. Landing queue length: {len(self.landing_queue)}.", 1)
            elif plane.condition == 'emergency':
                # Emergency planes must be prepended (disregarding queue FIFO, but respecting other emergency planes FIFO)
                # They are inserted after the last emergency planes (if any) in the queue
                insert_index = 0
                for i, enum_plane in enumerate(self.landing_queue):
                    if enum_plane.condition == 'emergency':
                        insert_index = i + 1
                    else:
                        break # Stop the index increase at the first non-emergency plane
                self.landing_queue.insert(insert_index, plane)
                FN_Print(f"[DEBUG]: Flight {plane.flight_number} inserted in landing queue at position #{insert_index+1}. Landing queue length: {len(self.landing_queue)}.", 1)
    # Pops the first (index 0) plane from the relevant queue
    def dequeue(self):
        # Landing planes are always prioritized
        if self.landing_queue:
            return self.landing_queue.pop(0)
        # If landing queue is empty, takeoff queue gets evaluated
        elif self.takeoff_queue:
            return self.takeoff_queue.pop(0)
        else:
            return None
    # Returns the total number of planes in both queues
    def total(self):
        return len(self.landing_queue) + len(self.takeoff_queue)

# --- Progress Bar Function
# Shows a progress bar depicting the current plane during its landing or takeoff operation.
def FN_ProgressBar(duration, direction, flight_number, useful_segments=50):
    # Dictionary of the symbols
    symbols = {
        'wall': '|',      # Wall symbol
        'left': '<',      # Plane symbol moving left
        'runway': '=',    # Runway symbol
        'right': '>'      # Plane symbol moving right
    }
    # Calculate the sleep duration for each segment
    sleep_duration = duration / useful_segments
    # Loop through each segment to simulate the plane progress
    for i in range(useful_segments + 1):
        percent_completed = i / useful_segments # Calculate the percentage of completion
        filled_segments = int(percent_completed * useful_segments) # Calculate how many segments are filled already
        remaining_segments = useful_segments - filled_segments # Calculate how many segments remain
        # Build the progress bar string, based on plane direction
        if direction == 'right':
            # Plane moves from left to right
            bar_string = ((filled_segments * symbols['right']) + (remaining_segments * symbols['runway']))
            # Inject flight number into the string
            insert_pos = min(filled_segments, useful_segments - len(str(flight_number)))
            bar_string = (bar_string[:insert_pos] + str(flight_number) + bar_string[insert_pos + len(str(flight_number)):])
        elif direction == 'left':
            # Plane moves from right to left
            bar_string = ((remaining_segments * symbols['runway']) + (filled_segments * symbols['left']))
            # Inject flight number into the string
            insert_pos = max(0, useful_segments - filled_segments)
            bar_string = (bar_string[:insert_pos] +  str(flight_number) + bar_string[insert_pos + len(str(flight_number)):])
        # Place walls in front  and print it
        bar_string_final = symbols['wall'] + bar_string[:useful_segments] + symbols['wall']
        print(f"\r[AIRWAY]: {bar_string_final}", end='', flush=True)
        # Wait before the next segment
        time.sleep(sleep_duration)
    # Move to the next line after the progress bar completes
    print()

# --- Plane Spawn
# Generates a plane with random attributes and enqueues it.
def FN_GeneratePlane():
    # Create and enqueue a plane with random attributes.
    while True:
        flight_number = random.randint(100, 999) # Flight number: 100–999
        if flight_number not in used_flight_numbers:
            used_flight_numbers.add(flight_number)
            break
    requested_action = random.choice(['landing', 'takeoff']) # Action: landing or takeoff
    # Randomly decide if the condition is normal (landing and takeoff) or emergency (landing only)
    if requested_action == 'landing':
        if random.random() < plane_emergency_chance:
            condition = 'emergency'
        else:
            condition = 'normal'
    elif requested_action == 'takeoff':
        condition = 'normal'
    # Spawn the plane
    plane = CL_Plane(flight_number, requested_action, condition)
    # Announce the plane's request
    if condition == 'emergency':
        FN_Print(f"[PLANE]: Flight {flight_number}, requesting {condition} {requested_action}!", 0)
    else:
        FN_Print(f"[PLANE]: Flight {flight_number}, requesting {requested_action}.", 0)
    # Enqueue the plane
    global_queue.enqueue(plane)
    known_planes.append(plane)

# --- Display statistics about the simulation
# Displays average wait times for normal and emergency landings, and takeoffs.
def FN_ShowStatistics():
    normal_landing_wait_times = []
    emergency_landing_wait_times = []
    takeoff_wait_times = []
    # Gather times
    for plane in known_planes:
        if plane.requested_action == 'landing' and plane.condition == 'normal':
            normal_landing_wait_times.append(plane.wait_duration)
        elif plane.requested_action == 'landing' and plane.condition == 'emergency':
            emergency_landing_wait_times.append(plane.wait_duration)
        elif plane.requested_action == 'takeoff':
            takeoff_wait_times.append(plane.wait_duration)
    # Calculate averages
    average_normal_landing_wait_time = sum(normal_landing_wait_times) / len(normal_landing_wait_times) if normal_landing_wait_times else 0
    average_emergency_landing_wait_time = sum(emergency_landing_wait_times) / len(emergency_landing_wait_times) if emergency_landing_wait_times else 0
    average_takeoff_wait_time = sum(takeoff_wait_times) / len(takeoff_wait_times) if takeoff_wait_times else 0
    # Print summary
    FN_Print(f"[INFO]: Average wait time for normal landings: {average_normal_landing_wait_time:.2f} seconds", 0)
    FN_Print(f"[INFO]: Average wait time for emergency landings: {average_emergency_landing_wait_time:.2f} seconds", 0)
    FN_Print(f"[INFO]: Average wait time for takeoffs: {average_takeoff_wait_time:.2f} seconds", 0)
    # Debug full details
    FN_Print("[DEBUG]: Final condition of all planes:", 1)
    for plane in known_planes:
        FN_Print(f"[DEBUG]: Flight {plane.flight_number}: {plane.requested_action}, Condition: {plane.condition}, Status: {plane.status}, Waited: {plane.wait_duration:.2f}sec", 1)

# --- Main Simulation Loop
# Planes spawn and request actions, the control tower responds, queues and processes requests based on conditions.
# Simulation parameters
program_version = "1.4" # Program version
global_queue = CL_Queue() # Queue Spawn
total_planes = 24 # Total number of planes to simulate
plane_emergency_chance = 0.35 # Percent chance of a plane declaring emergency
known_planes = [] # Local list to keep track of all planes that have requested actions
used_flight_numbers = set() # A set that ensures unique flight numbers
control_status_delay_min_sec = 0.1 # Defines fast response
control_status_delay_max_sec = 1.0 # Defines slow response
plane_takeoff_duration_min_sec = 1.0 # Defines fast takeoff
plane_takeoff_duration_max_sec = 3.0 # Defines slow takeoff
plane_landing_duration_min_sec = 2.0 # Defines fast landing
plane_landing_duration_max_sec = 3.0 # Defines slow landing
plane_emergency_extra_delay = 1.0 # Defines slower emergency landing
concurrent_plane_spawn_min = 1 # Minimum concurrent plane spawn in a given batch
concurrent_plane_spawn_max = 4 # Maximum concurrent plane spawn in a given batch
batch_index = 1 # Plane batches processed
spawned_planes = 0 # Successfully spawned planes counter
show_debug = False # Debug Flag
FN_Print(f"[INFO]: Starting \"IMT-Airport-Manager\" (v{program_version})...", 0)
# Determine Debug mode
while True:
    user_input = str(input("[INPUT]: Do you want to show Debug messages? [Y/(N)]: "))
    if user_input in ('y', 'Y'):
        show_debug = True
        break
    elif user_input in ('n', 'N', ''):
        show_debug = False
        break
    else:
        print("[INPUT]: Please press \"Y\" for Yes, \"N\" or Enter for No.")
# Primary execution loop
while spawned_planes < total_planes:
    FN_Print(f"[DEBUG]: Running batch #{batch_index}.", 1)
    batch_index += 1
    # Determine how many planes will spawn during this batch
    if spawned_planes < total_planes:
        planes_to_spawn = min(random.randint(concurrent_plane_spawn_min, concurrent_plane_spawn_max), (total_planes - spawned_planes))
        FN_Print(f"[DEBUG]: Spawning {planes_to_spawn} planes.", 1)
        for _ in range(min(planes_to_spawn, total_planes - spawned_planes)):
            FN_GeneratePlane()
            spawned_planes += 1
    # Determine control tower delay
    delay = random.uniform(control_status_delay_min_sec, control_status_delay_max_sec)
    time.sleep(delay)
    # Determine how many planes to handle this time (therefore imitating threading in a sense)
    planes_to_handle = random.randint(1, max(1, global_queue.total()))
    FN_Print(f"[DEBUG]: Control will handle {planes_to_handle} planes.", 1)
    for _ in range(planes_to_handle):
        current_plane = global_queue.dequeue()
        if current_plane:
            current_plane.status = 'proceed'
            current_plane.wait_duration = time.time() - current_plane.request_time
            FN_Print(f"[DEBUG]: Flight {current_plane.flight_number} waited {current_plane.wait_duration:.2f} seconds.", 1)
            FN_Print(f"[DEBUG]: Airway reserved for flight {current_plane.flight_number}.", 1)
            if current_plane.condition == 'emergency':
                FN_Print(f"[CONTROL]: Acknowledged Flight {current_plane.flight_number}, you have been given priority!", 0)
                FN_Print(f"[CONTROL]: Flight {current_plane.flight_number}, you are clear for {current_plane.condition} {current_plane.requested_action}.", 0)
            else:
                FN_Print(f"[CONTROL]: Flight {current_plane.flight_number}, you are clear for {current_plane.requested_action}.", 0)
            # Determine landing duration
            if current_plane.requested_action == 'landing':
                operation_delay = random.uniform(plane_landing_duration_min_sec, plane_landing_duration_max_sec)
                # Add emergency delay if applicable
                if current_plane.condition == 'emergency':
                    operation_delay += plane_emergency_extra_delay
                direction = 'left'
            # Determine takeoff duration
            else:
                operation_delay = random.uniform(plane_takeoff_duration_min_sec, plane_takeoff_duration_max_sec)
                direction = 'right'
            # Display the progress bar
            FN_ProgressBar(operation_delay, direction=direction, flight_number=current_plane.flight_number)
            current_plane.status = 'success'
            FN_Print(f"[DEBUG]: Airway clear.", 1)
        else:
            FN_Print(f"[INFO]: No more planes in queue.", 0)
FN_ShowStatistics()
