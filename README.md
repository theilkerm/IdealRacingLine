# Finding Ideal Racing Line with Evolutionary Artificial Neural Networks

A 2D Unity simulation in which cars learn to navigate themselves through different courses. The cars are steered by a feedforward Neural Network. The weights of the network are trained using a modified genetic algorithm.

I forked and made changes on it to find ideal racing line in karting.

## Usage

For now I'm working towards getting better lap times by lowering the MAX_CHECKPOINT_DELAY in carController.cs [UnityProject/Assets/Scripts/Simulation/CarController.cs](UnityProject/Assets/Scripts/Simulation/CarController.cs). Basically, if we say "n" to the number of checkpoints defined in unity,
max lap time= (n-1)\*MAX_CHECKPOINT_DELAY
we will get results.

I added a Time.timeScale value to [UnityProject/Assets/Scripts/Simulation/TrackManager.cs](UnityProject/Assets/Scripts/Simulation/TrackManager.cs) to see it faster as it sometimes takes hundreds of generations to see. on my system it gives nice and visible results over 32x.
Of course, in order to do these, you will need to model the runway via unity.

Short demo video of an early version: https://youtu.be/rEDzUT3ymw4

Movement constants

for 6.5hp rental karts in tuzla
private const float MAX_VEL = 20f;
private const float ACCELERATION = 8f;
private const float VEL_FRICT = 2f;
private const float TURN_SPEED = 100;

    can be found in CarMovement.cs

## Roadmap

1. Momentarily showing the best lap time on the screen.

2. After a successful round, the MAX_CHECKPOINT_DELAY value is reduced by a certain amount and a new target is set and the search for the perfect round continues.

3. Displaying the race line of the fastest lap in color on the screen with TrailRenderer.

4. Coloring the race line according to acceleration or speed after the 3rd stage has been successfully completed.

![](Images/Demo.gif)

## The Simulation

Cars have to navigate through a course without touching the walls or any other obstacles of the course. A car has five front-facing sensors which measure the distance to obstacles in a given direction. The readings of these sensors serve as the input of the car's neural network. Each sensor points into a different direction, covering a front facing range of approximately 90 degrees. The maximum range of a sensor is 10 unity units. The output of the Neural Network then determines the car’s current engine and turning force.

<img src="Images/Car.png" width="250">

If you would like to tinker with the parameters of the simulation, you can do so in the Unity Editor. If you would simply like to run the simulation with default parameters, you can start the built file [Builds/Applying EANNs.exe](Builds/Applying EANNs.exe).

## The Neural Network

The Neural Network used is a standard, fully connected, feedforward Neural Network. It comprises 4 layers: an input layer with 5 neurons, two hidden layers with 4 and 3 neurons respectively and an output layer with 2 neurons.
The code for the Neural Network can be found at [UnityProject/Assets/Scripts/AI/NeuralNetworks/](UnityProject/Assets/Scripts/AI/NeuralNetworks/).

## Training the Neural Network

The weights of the Neural Network are trained using an Evolutionary Algorithm known as the Genetic Algorithm.

At first there are N randomly initialised cars spawned. The best cars are then selected to be recombined with each other, creating new "offspring" cars. These offspring cars then form a new population of N cars and are
also slightly mutated in order to inject some more diversity into the population. The newly created population of cars then tries to navigate the course again and the process of evaluation, selection, recombination and mutation starts again. One complete cycle from the evaluation of one population to the evaluation of the next is called a generation.

The generic version of a Genetic Algorithm can be found at [UnityProject/Assets/Scripts/AI/Evolution/GeneticAlgorithm.cs](UnityProject/Assets/Scripts/AI/Evolution/GeneticAlgorithm.cs). This class can be modified in a very easy way, by simply assigning your own methods to the delegate operator methods of the class. Some example code for adapting the Genetic Algorithm to your own needs can be found in the EvolutionManager [UnityProject/Assets/Scripts/AI/Evolution/EvolutionManager.cs](UnityProject/Assets/Scripts/AI/Evolution/EvolutionManager.cs), which is already able to switch between two differently modified Genetic Algorithms.

## User Interface

The user interface always displays the data of the current best car. In the top left corner the Neural Network's output (engine and turning) is displayed. Right below the output, the evaluation value is displayed (the evaluation value is equal to the percentage of course completion). In the lower left corner a generation counter is displayed. In the upper right corner the Neural Network of the current best car is displayed. The weights are symbolised by the color and width of the connections between neurons: The wider a connection, the bigger the absolute value of the weight; Green means that the weight is positive, red means that the weight is negative.

The entire UI-code is located at [UnityProject/Assets/Scripts/GUI/](UnityProject/Assets/Scripts/GUI/).

## Courses

There are multiple courses of different difficulties which are all located in different unity scenes and can be found in the folder [UnityProject/Assets/Scenes/Tracks/](UnityProject/Assets/Scenes/Tracks/).

In order to start the simulation on a specific course, open the Main scene and enter the desired track-name (= scene name) in the Inspector of the GameStateManager object.

![Two different courses the cars can be trained on.](Images/Courses.png)

## License

Feel free to use my code in your personal projects. I would be very interested in any work that originates from this project. I would be more than happy to hear from your impressions and results, so feel free to mail me at arzt.samuel@live.de.
You can also follow me on twitter: https://twitter.com/SamuelArzt
