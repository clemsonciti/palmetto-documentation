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

![1](https://drive.google.com/file/d/1ePayDe_RqXAbQPTEozgQ7eZs8vNTl5Eu/view?usp=sharing)

Then select the required hardware for your simulation. As your models get bigger and more complex you should request more hardware.

![2](https://drive.google.com/file/d/1tMpU4IVKEc2wNOE03pfOBZ-Dhjilub76/view?usp=sharing)

Be sure to set the resolution to a configuration to what will sit comfortably on your screen, and to give yourself enough wall time to fully set up, run and review your simulation.

![3](https://drive.google.com/file/d/1N9LZzpzWnVQtIMAnkoKjj70fcN4OESkE/view?usp=sharing)

Once your hardware is selected, click ‘Launch’
Your request will be submitted to the queue, and you may have to wait  for the hardware to become available depending on how much you requested.

![4](https://drive.google.com/file/d/1Hav7TIjTpErgXJTd0PuxXZm_DTVpEX_d/view?usp=sharing)

Once the job begins you will be able to adjust compression and image quality and then launch the application.

![5](https://drive.google.com/file/d/1-U9Qyyu8Utn3EtDgJ2GKSACNU0UQRTHa/view?usp=sharing)

The following is a tutorial for running a simple FEA simulation of a cantilevered beam in ABAQUS

![6](https://drive.google.com/file/d/1iLr3Z9obp8goq0Khk7OdA-UnDOqF_LVa/view?usp=sharing)

Create your model database ‘with standard/explicit model’ to create a new simulation, you can also run a scripted simulation or open an existing file.

Double click on ‘Part’ to create a new model, for this simulation we will model a simple beam by sketching a rectangle that will be extruded. This part will be a 3D, deformable, solid created from an extrusion. Abaqus does not work with units, so before setting up a simulation you will need to make sure you’ve converted all of your variables to an agreeing unit system

![7](https://drive.google.com/file/d/1V14U3U_OohAAmhDfdSP860paPjsD6Ckl/view?usp=sharing)

In the toolbar on the left side of the design window, double click on a rectangle to create a rectangular sketch from two points. Click the centroid of the design window to start the rectangle and click a second point to finish the shape.

![8](https://drive.google.com/file/d/1_KcR7yPCb0Q_JmbcSXlkO43oB8eEY5dt/view?usp=sharing)

In the design toolbar click the dimension icon and then the side of the rectangle that you would like to dimension and type in the length of the line, repeat this for the adjacent side

![9](https://drive.google.com/file/d/1PGylmDUd8wXh9HKauODwsY4oENKpK45g/view?usp=sharing)

When you are finished with the sketch click ‘done’ in the bottom toolbar and input the width of the part in the pop-up window.

![10](https://drive.google.com/file/d/1t6mvPrpDfx2_OSrjTwPY3qB53q2sQuM2/view?usp=sharing)

The next step is to create a section assignment, expand ‘Parts’ and ‘Part-1’ in the design tree and double click ‘section assignment’. Click-and-drag across the design window to select the entire part. In the bottom bar name the new set ‘beam’ . Sets in Abaqus are just a way to identify different parts in a model.

![11](https://drive.google.com/file/d/17qfB_k5Bd3zLJhLMauhWzU7eY7FQR9Tc/view?usp=sharinghttps://drive.google.com/drive/u/0/folders/1DQWwnugrOzHxVhgWGJ13YfonuoOliire)

When you click ‘done’ the ‘Edit section Assignment’ pop up will appear, on this pop up you can click the I icon to create a new section. This section will be a homogeneous solid. ‘Continue’ and the next pop up will ask you to define the material for the section, you also have the option to create a new material by clicking the stress/strain icon

![12](https://drive.google.com/file/d/1Es6ek5aOu-lq4FdDdHeR0eK0i8CMiUCM/view?usp=sharing)

In the next menu you can input the name, description and relevant variables for your new material. This simulation is run with an imaginary material so the Young’s modulus and poisson ratio are somewhat arbitrary.

![13](https://drive.google.com/file/d/1y1BG_b21T_kj1gL4Yvc8xFJr-mHdNQcY/view?usp=sharing)

Press okay and in the original pop up make sure the correct section and material are selected, and click okay.

The next step is to mesh the part. In the design tree, expand ‘Parts’ and double click on ‘mesh’14
In the top left corner of the design toolbar, click the ‘seed part’ icon. This is used to control the general size of mesh elements. Change the settings in the pop up window and click ‘apply’ to see the effect of the change. When the mesh seeds are an appropriate distance from each other click ‘OK’.

![14](https://drive.google.com/file/d/1cjhlsZfaKjuD0qY-jBSvwkl9OebGNWWU/view?usp=sharing)

To actually mesh the part, double click on the mesh icon in the toolbar, and click okay. This could take a few minutes depending on your hardware and the size/complexity of your part.

![15](https://drive.google.com/file/d/1l6KBhTAqaZCiZp2KoJ9ezfrOYE_IActs/view?usp=sharing)

Once meshing is complete, scroll down in the design tree and expand ‘Assembly’. Double click on instances to create an instance. In assemblies, instances are used to define how different parts are connected to each other. In this simulation we are analyzing a single part, select ‘part-1’ in the pop up window (it should be the only option) and click OK.

![16](https://drive.google.com/file/d/1huhZ8TBbn-_1Z045U6mqU0yuctFNotDF/view?usp=sharing)

Then navigate to ‘Steps’ in the design tree and double click to create a Step, we will name this step ‘load’ and in this step we will apply a connection and a force to our beam. The new step should be inserted after the initial step as a ‘static, general’ procedure. Click ‘continue’

![17](https://drive.google.com/file/d/1j8wQn82f_2StM1BfZRpQaVg3og4X7xQA/view?usp=sharing)

On the next pop up add a step description and click okay, then double click on ‘BCs’ in the design tree to create a new boundary condition.
For this simulation our only boundary condition will be one side of the beam that is completely fixed. The assigned step should be initial,and the Type will be ‘Symmetry/Antisymmetry/Encastre’.
Click continue

![18](https://drive.google.com/file/d/1FeKm_NvemVJWiEeKYi0ed6IpV5Dy9pga/view?usp=sharing)

The ‘Region selection’ pop up can be dismissed because we want to select a single face, not the whole part.

![19](https://drive.google.com/file/d/108o30-FLpBDcmN4o63PtSf8N-yiMP8mv/view?usp=sharing)

Next we will rotate the part in the design window so that we can see the part we would like to fix.
Click the face and it’s color will change to red. At the bottom bar ‘Create set’ should be selected, name the new set ‘support-set’ and click ‘Done’

![20](https://drive.google.com/file/d/15ZsTQ9kn2nakf0TpiqhNEveLHFPtqEXT/view?usp=sharing)

ON the next pop up, select ‘ENCASTRE’ to fix each nodal point. With the other options the part will be allowed to translate or rotate about an axis. Click okay.

![21](https://drive.google.com/file/d/1UgGwN9hmnlJQHn1gjR39bytfotxRrZLU/view?usp=sharing)

You should be able to see that each nodal point has an indicator that it is fixed.

![22](https://drive.google.com/file/d/1ZQjtSaq5Dxh9o1jS_4KskkS321L24fr1/view?usp=sharing)

Next, we will define the load that we will apply to the part. In the design tree double click on ‘Loads’ . Name the load and change its assigned Step to the ‘Load’ step that we created earlier, category should be mechanical and the type will be pressure. Click Continue

![23](https://drive.google.com/file/d/1p1pgCcZElet0JzovCBsgNcu620mcI7uJ/view?usp=sharing)

Now to select the surface to which we will apply the force, click on the top of the beam in the design window. It will recolor. On the bottom bar rename the new surface ‘load surface’ and click done.

![24](https://drive.google.com/file/d/1u3SwHzoUixZlCfrcmDcWdLaAqZ6ANO01/view?usp=sharing)

You will be able to see the applied load represented as arrows pointing downwards across the selected surface.

![25](https://drive.google.com/file/d/1A7Yvdlp9sThQdpPvlOoPnPlNR1wUO1j6/view?usp=sharing)

In the next pop up box  input the desired magnitude of the applied load.

Now we need to define what kind of output we want from the simulation. In the design tree expand field output request,and double click ‘F-output’  , in the next pop up you can customize what reports you would like from the simulation.

![26](https://drive.google.com/file/d/1XVixfyGAYejSOW5zGFkyzIHXwJX6gEQ-/view?usp=sharing)

The next-to-last step is to create and run the job. Double click on jobs in the design tree to create a new job. Name the job and select the model that will be analysed, since we only have one part it should be the only option.

![27](https://drive.google.com/file/d/1XVixfyGAYejSOW5zGFkyzIHXwJX6gEQ-/view?usp=sharing)

The next pop up contains more options to customize how the job executes. For this simulation we will simply add a description and click okay.

![28](https://drive.google.com/file/d/17C56x1dB3DsbBSncRiAGQ9D8Xnpe33_p/view?usp=sharing)

Now to run the simulation, expand jobs in the design tree and right

![29](https://drive.google.com/file/d/1XVixfyGAYejSOW5zGFkyzIHXwJX6gEQ-/view?usp=sharing)

.click on your job. Click submit.
In the bottom window you will see a message when the job is completed









