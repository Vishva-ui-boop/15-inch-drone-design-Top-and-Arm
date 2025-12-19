// ================================
// PARAMETRIC DRONE FRAME – 15 INCH
// Square box + 4 angled arms
// ================================

// -------- PARAMETERS --------
box_size = 120;        // center box size (mm)
box_height = 60;       // box height
plate_thk = 3;         // top & bottom plate thickness

arm_size = 20;         // square arm size
arm_length = 180;      // arm length
arm_insert = 25;       // arm insertion into box
arm_angle = 30;        // arm angle outward (degrees)

motor_plate = 45;
motor_plate_thk = 4;

// -------- MODULES --------
module center_box() {
    difference() {
        // outer box
        cube([box_size, box_size, box_height], center=true);

        // hollow inside
        translate([0,0,0])
        cube([box_size-20, box_size-20, box_height-6], center=true);
    }
}

module arm() {
    cube([arm_length, arm_size, arm_size], center=false);
}

module motor_plate() {
    translate([arm_length, arm_size/2, arm_size/2])
        cube([motor_plate, motor_plate, motor_plate_thk], center=true);
}

// -------- ASSEMBLY --------
module arm_assembly(rotation) {
    rotate([0,0,rotation])
    translate([box_size/2, -arm_size/2, -arm_size/2])
    rotate([0,0,arm_angle])
    arm();
}

// -------- DRAW --------
union() {

    // Center box
    center_box();

    // Arms (4 sides)
    arm_assembly(0);
    arm_assembly(90);
    arm_assembly(180);
    arm_assembly(270);
}
