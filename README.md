# virtual_costmap_layer

This package includes a plugin to add a virtual layer of obstacles and to define a navigation zone in the 2D costmap. 
The user can define several geometric forms (point, line, circle, polygon) in the map frame.   

1. **Point**: geometry_msgs::Point [x, y, z=0]
2. **Line**:  2 * geometry_msgs::Point[x, y, z=0]
3. **Circle**: geometry_msgs::Point[x, y, z>0]
4. **Polygon with n edges**: n * geometry_msgs::Point[x, y, z=0]

The plugin subscribes to different topics for zone and for obstacles to receive data. It is also possible to define all those forms in the config YAML file.

![Presentation](/demo/presentation.gif "Presentation")

## Example

1. Clone the repository into your ROS workspace `catkin_ws/src/`
2. Build the workspace using `catkin_make`
3. Source the workspace: `source devel/setup.bash`
4. Launch the demo: `roslaunch virtual_costmap_layer sample.launch`
5. Use command-line to post obstacles and zones into following topics:
   - `/virtual_costmap_layer/obstacles` for obstacles
   - `/virtual_costmap_layer/zones` for zones
   ```bash
   rostopic pub /virtual_costamp_layer/obsctacles virtual_costmap_layer/Obstacles "list: [{form: [{x: 0.4, y: 0.0, z: 0.0}]}, {form: [{x: 0.0, y: -0.5, z: 0.0}]}]" # Points, each form contains exactly one point
   rostopic pub /virtual_costamp_layer/obsctacles virtual_costmap_layer/Obstacles "list: [{form: [{x: 0.4, y: 0.0, z: 1.0}]}]" # Circle, each form contains exactly one point with z > 0
   rostopic pub /virtual_costamp_layer/obsctacles virtual_costmap_layer/Obstacles "list: [{form: [{x: 0.4, y: 0.0, z: 0.0}, {x: 0.0, y: 0.4, z: 0.0}]}]" # Line, each form object contains exactly two points
   rostopic pub /virtual_costamp_layer/obsctacles virtual_costmap_layer/Obstacles "list: [{form: [{x: 0.4, y: 0.0, z: 0.0}, {x: 0.0, y: 0.5, z: 0.0}, {x: -0.4, y: 0.0, z: 0.0}, {x: 0.0, y: -0.5, z: 0.0}]}]" # Polygon, each form object contains at least three points
   ```
6. Watch the results in opening RViz and visualize the costmap layer.

## Known Issues (Update: 2025-08-04)

- Loading points and polygons from `forms` section in the YAML file is buggy (at least in master branch). Consider using `rostopic pub` command to post obstacles and zones.
- Line generation will cause a crash so kindly avoid using it.