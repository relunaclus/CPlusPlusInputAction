# Environment Verification Task

---

## Introduction

For this project, I was tasked with the creation of a C++ Unreal Project, to compile it successfully, and to highlight a class and a property or function within C++ in Unreal. To do this I utilised Unreal's Documentation to successfully create a C++ class representing a slowly rotating actor that also moves up and down, similar to an item pickup in many games. This was an important task to do as my first introduction to using C++ within Unreal, and helped with my understanding of its implementation within the engine. I utilised a separate blueprint class that inherits from the C++ class using a basic cube mesh in order to visually represent the actions taken by the C++ class. This is helpful as Blueprints and Unreal C++ are designed with the intention of being used together for the most efficient and effective workflow. 

*Figure 1: A gif of a rotating cube in Unreal Engine*
![A gif of a rotating cube in Unreal Engine](./README%20Assets/rotatingcube.gif "Rotating Cube")

---

## Implementation

For this project I first used created a C++ class and a Header class for the C++ class to inherit from, to complete this task I utilised an online tutorial to create a rotating cube. Using Visual Studio 2022, and C++ in Unreal, I was able to follow the steps to create a cube.

To begin with, I created an Actor C++ Class in Unreal, I then moved to Visual Studio by opening the solution file in order to edit the class and its header file.  

*Figure 2: A screenshot of Visual Studio 2022*  
![A screenshot of Visual Studio 2022](./README%20Assets/VS22F.png "Classes")  
I then opened the header file "MyClass.h" and added the following code underneath the class declaration for AMyClass

```
    UPROPERTY(VisibleAnywhere)
    UStaticMeshComponent* VisualMesh;
```

I then opened MyClass.cpp and added this code within the constructor, AMyClass::AMyClass()

```
	VisualMesh = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Mesh"));
	VisualMesh->SetupAttachment(RootComponent);

	static ConstructorHelpers::FObjectFinder<UStaticMesh> CubeVisualAsset(TEXT("/Game/StarterContent/Shapes/Shape_Cube.Shape_Cube"));

	if (CubeVisualAsset.Succeeded())
	{
		VisualMesh->SetStaticMesh(CubeVisualAsset.Object);
		VisualMesh->SetRelativeLocation(FVector(0.0f, 0.0f, 0.0f));
	}
```
I then  added this code within the tick function, so that the actor will continously rotate while also bobbing vertically.

```
	FVector NewLocation = GetActorLocation();
	FRotator NewRotation = GetActorRotation();
	float RunningTime = GetGameTimeSinceCreation();
	float DeltaHeight = (FMath::Sin(RunningTime + DeltaTime) - FMath::Sin(RunningTime));
	NewLocation.Z += DeltaHeight * 20.0f;       
	float DeltaRotation = DeltaTime * 20.0f;	
	NewRotation.Yaw += DeltaRotation;
	SetActorLocationAndRotation(NewLocation, NewRotation);
```
I then built the class and moved back to Unreal to check. When I placed the Actor in the World it did not have a mesh, which meant I could not visualise it to see if it was rotating, so I created a Blueprint Class inheriting from the C++ Class, and gave it a Cube mesh to visualise the movement.

When I pressed play in Unreal, the cube was rotating and bobbing up and down correctly, as it should have been.

*Figure 3: An image of a cube in Unreal Engine*  
![](./README%20Assets/rotatingcube.png "")



---

## Outcome

The final result of this has a cube that rotates and bobs up and down, this meets the requirements of the task as I have successfully compiled a C++ Class in Unreal Engine, and have modified its Vector and Rotation Properties within the Tick Function.
*Figure 4: A gif of a rotating cube in Unreal Engine*
![A gif of a rotating cube in Unreal Engine](./README%20Assets/rotatingcube.gif "Rotating Cube")
This gif clearly shows that after creating a Blueprint Class that inherits from the C++ Class, the cube rotates and moves vertically similar to a typical pickup in a videogame.

---

## Bibliography

Unreal Engine CPP Quick Start | Unreal Engine 5.7 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/unreal-engine/unreal-engine-cpp-quick-start (Accessed  15/04/2026).
Verhoeff, S. (2026) Setting up Unreal Engine for C++ Development in 2026. At: https://medium.com/@sophia.verhoeff/setting-up-unreal-engine-for-c-development-in-2026-fcd56cd26392 (Accessed  15/04/2026).

## AI Usage Declaration

Copilot was used for the purposes of IntelliSense to assist with coding.
