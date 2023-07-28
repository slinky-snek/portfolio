---
title: "Balloon Boppers"
last_modified_at: 2023-07-26T16:20:02-05:00
categories:
  - Games
tags:
  - Balloon Boppers
---

Balloon Boppers is a 2 to 4-person split-screen arena shooter in which the players are continuously falling. You only move around the arena using a grappling hook.
To win, you must have the most kills by the end of the timed match. Along with fighting others, you must stay in the arena to stay alive. 

Developed in Unreal Engine 5 by myself and one artist.
<br>
<h2>
Small Trailer
</h2>
<iframe width="560" height="315" src="https://www.youtube.com/embed/j3D1zjrDhuQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<h2>
Gameplay Video
</h2>
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZtpJay29BUw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<br>
<iframe frameborder="0" src="https://itch.io/embed/2166883" width="560" height="167"><a href="https://twixel.itch.io/balloon-boppers">Balloon Boppers by twixel</a></iframe>
<br>
<br>
<br>
<h1>
Some highlights:
</h1>
<h2>
Character Setup
</h2> 
Some snippets from the character and attribute set classes. This shows how delegates were setup as hooks for the UI to update. Also, Blueprint hooks were setup so classes deriving from the character could be notified of attribute changes. Full classes can be found on my GitHub in the GASCharacter repository.
<br>
<br>
<br>

This is the attribute and the delegate that will notify our character class that health has changed.
```cpp
// BoppersAttributeSet.h

DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams(FAttributeChangedDelegate, float, AttributeValue, int32, StackCount);

UPROPERTY()
FAttributeChangedDelegate OnHealthChangeDelegate;

UPROPERTY(BlueprintReadOnly, Category = "Attributes", ReplicatedUsing = OnRep_Health)
FGameplayAttributeData Health;
ATTRIBUTE_ACCESSORS(UBoppersAttributeSet, Health)
```
We'll broadcast from the attribute set delegate when the attribute is changed.
```cpp
// BoppersAttributeSet.cpp

void UBoppersAttributeSet::PostGameplayEffectExecute(const FGameplayEffectModCallbackData& Data)
{
	Super::PostGameplayEffectExecute(Data);
	if(Data.EvaluatedData.Attribute == GetHealthAttribute())
	{
		SetHealth(FMath::Clamp(GetHealth(), 0.f, GetMaxHealth()));
		OnHealthChangeDelegate.Broadcast(GetHealth(), Data.EffectSpec.StackCount);
	}
}
 ```
<br>
<br>
<br>

Here's an example of an attribute change hook that can be used in Blueprint classes deriving from this character class. This allows the Blueprint to be notified when an attribute has changed and maybe
do something about it. Below that is the native attribute changed event that we'll bind to the attribute set delegate above.
```cpp
// HeroCharacter.h

//Attribute change blueprint hook
UFUNCTION(BlueprintImplementableEvent, Category = "BoppersGameplayAbility")
void OnHealthChanged(float Health, int32 StackCount);

//Native attribute change event
UFUNCTION()
virtual void OnHealthChangedNative(float Health, int32 StackCount);
```
<br>
<br>
<br>

An example of an attribute change delegate we'll use to notify the UI to update itself. The UI will bind to this delegate when it's initialized.
```cpp
// HeroCharacter.h

DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams(FAttributeChangeDelegate, float, AttributeValue, int32, StackCount);

UPROPERTY(BlueprintAssignable, Category = "AttributeDelegates")
FAttributeChangeDelegate OnHealthChangeDelegate;
```
<br>
<br>
<br>

Here's where we bind the native function handler to the attribute set's change delegate we set up earlier. Below is where we broadcast from our native function handler when we receive a change event and also call the Blueprint hook.
```cpp
// HeroCharacter.cpp
void AHeroCharacter::BeginPlay()
{
	Super::BeginPlay();	
	if(AbilitySystemComponent)
	{
		AttributeSet = AbilitySystemComponent->GetSet<UBoppersAttributeSet>();
		const_cast<UBoppersAttributeSet*>(AttributeSet)->OnHealthChangeDelegate.AddDynamic(this, &AHeroCharacter::OnHealthChangedNative);
  	}
}

void AHeroCharacter::OnHealthChangedNative(float Health, int32 StackCount)
{
	OnHealthChangeDelegate.Broadcast(Health, StackCount);
	OnHealthChanged(Health, StackCount);
}
```
<br>
<br>
<br>
<h2>
Shooting Mechanic
</h2>
Below are some Blueprint snippets from the projectile gameplay ability and the character Blueprint classes. I implemented the shooting mechanic using Unreal's Gameplay Ability System. This allowed for quick iteration of possible pickups and more complex interactions with other mechanics.
<h3>
Projectile Gameplay Ability
</h3>
<iframe src="https://blueprintue.com/render/c9gumui_/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h3>
Triggering Character Ability
</h3>
<iframe src="https://blueprintue.com/render/jhq7s_h_/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<br>
<br>
<br>
<h2>
Grapple Mechanic
</h2>
Below are Blueprint snippets from the grapple component and character classes. I implemented an assist system for the grapple mechanic. The system finds the best location for the players grapple point nearest to where the player is aiming. This allowed grappling on controller to be much more accessible and fun. The player was able to focus more on where they wanted to move using the grapple and not on constantly trying to hit grapple targets precisely.
<h3>
Automatic Grapple Target Selection
</h3>
<iframe src="https://blueprintue.com/render/dh04g-nc/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h3>
Grapple Update
</h3>
<iframe src="https://blueprintue.com/render/5xjbls4h/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h3>
Grappled Movement Input Handling
</h3>
<iframe src="https://blueprintue.com/render/h8ihbz6e/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<br>
<br>
<br>
<h2>
Aim Assist Systems
</h2>
Below are Blueprint snippets from the aim assist component. Players are moving quickly around the arena and tracking on controller was too difficult even for experienced controller players. With the aim assist strategies below, all players were able to accurately score hits on others without feeling they were being aided too heavily. I implemented bullet magnetism, area cursor, sticky targets, and target gravity to test which would work best for the game. I settled on bullet magnetism and target gravity. With constantly moving targets, the sticky target approach would actually cause players to not be able to reach their target while trying to track them. For targets that don't move much or stop intermittently, this might be a good approach. Area cursor gave the player a feeling that they were hitting targets by cheating. Projectiles that fly directly at your target when they're "near" your reticle can feel off. The bullet magnetism approach worked quite well without players even knowing it was being used. Bullet magnetism gave players an increased chance that their shot would hit even with their target moving at high speeds. Target gravity helped the player track targets at high speed without giving them the feeling they were locked onto the target.
<h3>
Aim Assist Component (Main Graph)
</h3>
<iframe src="https://blueprintue.com/render/b2cdxey4/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h3>
Gravity Pull Subgraph
</h3>
<iframe src="https://blueprintue.com/render/p8gc2wi0/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h3>
Sticky Targets Subgraph
</h3>
<iframe src="https://blueprintue.com/render/7hsxg3cr/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<br>
<br>
<br>
<h2>
Sounds and Music
</h2>
I implemented all sound effects and music using MetaSounds. This allowed for greater control over when and how sounds were played. Included randomized music selection and pitch scaling of sound effects. Fewer sound effects were needed and each time one was played, it sounded different. This was especially important for the shooting, grappling, and impact sounds. Being able to make a metasound as a placeholder was also very helpful. You can play the empty metasound where you need it while building the mechanic. When you have a sound that you've picked later on, instead of having to find the call-site where you played the sound, you can edit the metasound asset.
<br>
<br>
<br>
<h2>
Input Handling
</h2>
If you're not using Unreal's Enhanced Input System, you should be. It offers a much better way to manage input contexts for your player via input mapping context assets. Also, the input modifiers are a great way to make sure your game doesn't feel wonky on controllers due to using raw input. You can create custom modifiers and there are even some built in that you can experiment with.
