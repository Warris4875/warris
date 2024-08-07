# balance_assist_model.py
from OCC.Core.BRepPrimAPI import BRepPrimAPI_MakeBox, BRepPrimAPI_MakeCylinder
from OCC.Core.gp import gp_Pnt, gp_Ax2, gp_Dir, gp_Trsf, gp_Vec
from OCC.Core.BRepBuilderAPI import BRepBuilderAPI_Transform
from OCC.Display.SimpleGui import init_display

# Initialize the display
display, start_display, add_menu, add_function_to_menu = init_display()

# Create the body (a tilted box to indicate imbalance)
body = BRepPrimAPI_MakeBox(1, 2, 5).Shape()
trsf = gp_Trsf()
trsf.SetRotation(gp_Ax2(gp_Pnt(0.5, 1, 0), gp_Dir(1, 0, 0)), 0.1)  # 0.1 radian tilt
transform = BRepBuilderAPI_Transform(body, trsf, True)
tilted_body = transform.Shape()

# Create the head (a cylinder)
head = BRepPrimAPI_MakeCylinder(gp_Ax2(gp_Pnt(0.5, 1, 5.5), gp_Dir(0, 0, 1)), 0.5, 1).Shape()

# Create the wearable device (a simple box representing a belt)
belt = BRepPrimAPI_MakeBox(3, 1, 0.5).Shape()
belt_position = gp_Trsf()
belt_position.SetTranslation(gp_Vec(0, 0.5, 2))
belt_transform = BRepBuilderAPI_Transform(belt, belt_position)
belt_shape = belt_transform.Shape()

# Create the gyroscope (a cylinder)
gyro = BRepPrimAPI_MakeCylinder(gp_Ax2(gp_Pnt(1.5, 0.75, 2.25), gp_Dir(0, 0, 1)), 0.25, 0.5).Shape()

# Display all parts
display.DisplayShape(tilted_body, update=True)
display.DisplayShape(head, update=True)
display.DisplayShape(belt_shape, update=True)
display.DisplayShape(gyro, update=True)

# Start the display
start_display()
