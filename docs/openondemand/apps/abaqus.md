## ABAQUS/CAE

ABAQUS is a Finite Element Analysis software used
for engineering simulations.
Currently, ABAQUS versions 6.14, 2018, 2020, and 2021 are available on Palmetto cluster
as modules.

~~~
$ module avail abaqus

abaqus/6.10 abaqus/6.13 abaqus/6.14
~~~

To see license usage of ABAQUS-related packages,
you can use the `lmstat` command:

~~~
/software/USR_LOCAL/flexlm/lmstat -a -c /software/USR_LOCAL/flexlm/licenses/abaqus.dat
~~~


### Running ABAQUS with OpenOD

To open an interactive session of ABAQUS/CAE, click on ‘Interactive apps’ and then ‘Abaqus/CAE’

![1](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/1.png)

Then select the required hardware for your simulation. As your models get bigger and more complex you should request more hardware.

![2](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/2.png)

Be sure to set the resolution to a configuration to what will sit comfortably on your screen, and to give yourself enough wall time to fully set up, run and review your simulation.

![3](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/3.png)

Once your hardware is selected, click ‘Launch’
Your request will be submitted to the queue, and you may have to wait  for the hardware to become available depending on how much you requested.

![4](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/4.png)

Once the job begins you will be able to adjust compression and image quality and then launch the application.

![5](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/5.png)

The following is a tutorial for running a simple FEA simulation of a cantilevered beam in ABAQUS

![6](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/6.png)

Create your model database ‘with standard/explicit model’ to create a new simulation, you can also run a scripted simulation or open an existing file.

Double click on ‘Part’ to create a new model, for this simulation we will model a simple beam by sketching a rectangle that will be extruded. This part will be a 3D, deformable, solid created from an extrusion. Abaqus does not work with units, so before setting up a simulation you will need to make sure you’ve converted all of your variables to an agreeing unit system

![7](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/7.png)

In the toolbar on the left side of the design window, double click on a rectangle to create a rectangular sketch from two points. Click the centroid of the design window to start the rectangle and click a second point to finish the shape.

![8](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/8.png)

In the design toolbar click the dimension icon and then the side of the rectangle that you would like to dimension and type in the length of the line, repeat this for the adjacent side

![9](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/9.png)

When you are finished with the sketch click ‘done’ in the bottom toolbar and input the width of the part in the pop-up window.

![10](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/10.png)

The next step is to create a section assignment, expand ‘Parts’ and ‘Part-1’ in the design tree and double click ‘section assignment’. Click-and-drag across the design window to select the entire part. In the bottom bar name the new set ‘beam’ . Sets in Abaqus are just a way to identify different parts in a model.

![11](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/11.png)

When you click ‘done’ the ‘Edit section Assignment’ pop up will appear, on this pop up you can click the I icon to create a new section. This section will be a homogeneous solid. ‘Continue’ and the next pop up will ask you to define the material for the section, you also have the option to create a new material by clicking the stress/strain icon

![12](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/12.png)

In the next menu you can input the name, description and relevant variables for your new material. This simulation is run with an imaginary material so the Young’s modulus and poisson ratio are somewhat arbitrary.

![13](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/13.png)

Press okay and in the original pop up make sure the correct section and material are selected, and click okay.

The next step is to mesh the part. In the design tree, expand ‘Parts’ and double click on ‘mesh’
In the top left corner of the design toolbar, click the ‘seed part’ icon. This is used to control the general size of mesh elements. Change the settings in the pop up window and click ‘apply’ to see the effect of the change. When the mesh seeds are an appropriate distance from each other click ‘OK’.

![14](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/14.png)

To actually mesh the part, double click on the mesh icon in the toolbar, and click okay. This could take a few minutes depending on your hardware and the size/complexity of your part.

![15](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/15.png)

Once meshing is complete, scroll down in the design tree and expand ‘Assembly’. Double click on instances to create an instance. In assemblies, instances are used to define how different parts are connected to each other. In this simulation we are analyzing a single part, select ‘part-1’ in the pop up window (it should be the only option) and click OK.

![16](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/16.png)

Then navigate to ‘Steps’ in the design tree and double click to create a Step, we will name this step ‘load’ and in this step we will apply a connection and a force to our beam. The new step should be inserted after the initial step as a ‘static, general’ procedure. Click ‘continue’

![17](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/17.png)

On the next pop up add a step description and click okay, then double click on ‘BCs’ in the design tree to create a new boundary condition.
For this simulation our only boundary condition will be one side of the beam that is completely fixed. The assigned step should be initial,and the Type will be ‘Symmetry/Antisymmetry/Encastre’.
Click continue

![18](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/18.png)

The ‘Region selection’ pop up can be dismissed because we want to select a single face, not the whole part.

![19](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/19.png)

Next we will rotate the part in the design window so that we can see the part we would like to fix.
Click the face and it’s color will change to red. At the bottom bar ‘Create set’ should be selected, name the new set ‘support-set’ and click ‘Done’

![20](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/20.png)

ON the next pop up, select ‘ENCASTRE’ to fix each nodal point. With the other options the part will be allowed to translate or rotate about an axis. Click okay.

![21](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/21.png)

You should be able to see that each nodal point has an indicator that it is fixed.

![22](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/22.png)

Next, we will define the load that we will apply to the part. In the design tree double click on ‘Loads’ . Name the load and change its assigned Step to the ‘Load’ step that we created earlier, category should be mechanical and the type will be pressure. Click Continue

![23](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/23.png)

Now to select the surface to which we will apply the force, click on the top of the beam in the design window. It will recolor. On the bottom bar rename the new surface ‘load surface’ and click done.

![24](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/24.png)

You will be able to see the applied load represented as arrows pointing downwards across the selected surface.

![25](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/25.png)

In the next pop up box  input the desired magnitude of the applied load.

Now we need to define what kind of output we want from the simulation. In the design tree expand field output request,and double click ‘F-output’  , in the next pop up you can customize what reports you would like from the simulation.

![26](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/26.png)

The next-to-last step is to create and run the job. Double click on jobs in the design tree to create a new job. Name the job and select the model that will be analysed, since we only have one part it should be the only option.

![27](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/27.png)

The next pop up contains more options to customize how the job executes. For this simulation we will simply add a description and click okay.

![28](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/28.png)

Now to run the simulation, expand jobs in the design tree and right click on your job. Click submit.

![29](https://github.com/clemsonciti/palmetto-documentation/blob/master/docs/images/ood/apps/ABAQUS/29.png)

In the bottom window you will see a message when the job is completed









