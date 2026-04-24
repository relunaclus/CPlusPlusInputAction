# Input Action Task

---

## Introduction

For this project, I was tasked with the implementation of one input action using Enhanced Input in C++ and to expose at least one parameter to Blueprint for tuning. To complete this task I used Unreal's Documentation in combination with assistance from my peers. This task is helpful as control of input systems are necessary to the development of a modular game. 
---

## Implementation

To begin with, I created a Character C++ Class in Unreal, I then moved to Visual Studio by opening the solution file in order to edit the class and its header file.  I opened the MyCharacter.h file to begin and declared the following functions in order to allow Input Action to work within Unreal.

```
	virtual void BeginPlay() override;
	virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category ="Input")
	UInputMappingContext* DefaultMappingContext;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Input")
	UInputAction* MoveAction;

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Movement")
	float MoveSpeed = 400.0f;
	
	void Move(const FInputActionValue& Value);
```
I then opened MyCharacter.cpp in order to add the mapping context to the player controller using the following code.

```
APlayerController* PC = Cast<APlayerController>(GetController())
```

I then added the SetupPlayerInputComponent which should then add the MoveAction to the Enhanced Input

```
void AMyCharacter::SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)
{
	Super::SetupPlayerInputComponent(PlayerInputComponent);
	if (UEnhancedInputComponent* EnhancedInputComponent = 
		Cast<UEnhancedInputComponent>(PlayerInputComponent))
	{
		EnhancedInput->BindAction(MoveAction, ETriggerEvent::Triggered, this, &AMyCharacter::Move);
	}
}
```

Moving was then defined using a pair of Movement Vectors, one for right and one for forward. This is based on where the player is looking by using the Controller's rotation

```
void AMyCharacter::Move(const FInputActionValue& Value)
{
	FVector2D MovementVector = Value.Get<FVector2D>();

	if (Controller)
	{
		const FRotator Rotation = Controller->GetControlRotation();
		const FRotator YawRotation(0, Rotation.Yaw, 0);

		const FVector DirectionX = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::X);
		const FVector DirectionY = FRotationMatrix(YawRotation).GetUnitAxis(EAxis::Y);

		AddMovementInput(DirectionX, MovementVector.Y;
		AddMovementInput(DirectionY, MovementVector.X);
	}
}
```

---

## Outcome

The final result allows for a player to control a basic character using the W, A, S, and D keys on the keyboard.

---

## Bibliography

Unreal Engine 5.6 Documentation | Unreal Engine 5.6 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/unreal-engine/unreal-engine-5-5-documentation (Accessed  24/04/2026).


## AI Usage Declaration

Copilot was used for the purposes of IntelliSense to assist with coding.
