## Gameplay Framework

Unreal Engine（UE）的游戏玩法框架（Gameplay Framework）是开发游戏时非常重要的一个部分。它提供了一套灵活且强大的工具和类，用于处理角色、控制器、关卡、HUD、游戏模式等游戏核心组件。以下是对UE游戏玩法框架的一些关键概念和类的详细解释：

### 关键类和概念

**AActor**：这是所有对象的基类，几乎所有可以放置在关卡中的对象都是从AActor派生的。它处理位置、旋转、缩放等基本属性。

**APawn**：从AActor派生，用于表示游戏中的角色或载具。Pawn可以由玩家或AI控制。

**ACharacter**：从APawn派生，专门用于角色的类。它增加了角色运动组件，用于处理行走、跳跃等功能。

**APlayerController**：控制玩家Pawn的类。它处理玩家输入，将输入转化为Pawn的动作。

**AGameModeBase**：管理游戏规则和状态的类。它决定了玩家的生成、游戏胜利或失败的条件等。

**AGameStateBase**：存储游戏的状态信息，如比赛时间、得分等。

**APlayerState**：存储单个玩家的状态信息，如名字、得分等。

**UUserWidget**：用于创建UI的基类。它通常与UMG（Unreal Motion Graphics）一起使用，来设计游戏的HUD、菜单等界面。

## GameModes

在Unreal Engine（UE）中，`GameMode` 类是游戏逻辑的核心，它定义了游戏规则、流程、玩家行为以及各种游戏元素的交互方式。`GameMode` 主要用于管理和控制以下内容：

### 1. 游戏规则和流程

`GameMode` 负责定义游戏的主要规则和流程，包括如何开始游戏、如何结束游戏以及如何处理游戏中的事件。它可以控制游戏的胜负条件、计分系统等。

### 2. 默认类

`GameMode` 指定了一些游戏中默认使用的类，包括：

- **玩家控制器类（PlayerController Class）**：控制玩家输入和行为。
- **Pawn类（Pawn Class）**：玩家控制的角色或载具。
- **HUD类（HUD Class）**：处理游戏界面的显示。
- **PlayerState类（PlayerState Class）**：存储每个玩家的状态信息。
- **GameState类（GameState Class）**：存储整个游戏的状态信息。

### 3. 玩家生成

`GameMode` 决定玩家在游戏中的生成位置和生成方式。它可以控制玩家角色的初始状态以及重新生成机制。

### 4. 游戏逻辑

`GameMode` 可以包含复杂的游戏逻辑，如关卡进度、任务管理、多人游戏中的队伍管理等。

### 5. 处理比赛和回合

对于比赛型游戏或回合制游戏，`GameMode` 负责管理比赛和回合的进程，包括计时器、回合切换和结果判定。

### 如何创建和使用GameMode

#### 1. 创建GameMode类

在Unreal Engine中创建一个新的GameMode类：

1. 打开Unreal Engine编辑器。
2. 在内容浏览器中，右键点击空白区域，选择 `Create` > `Blueprint Class`。
3. 在弹出的窗口中，选择 `GameModeBase` 作为父类。
4. 命名你的GameMode类，例如 `MyGameMode`。

#### 2. 配置GameMode类

在新创建的GameMode类中，你可以配置各种默认类和游戏规则：

1. 打开 `MyGameMode` 蓝图。
2. 在详细信息面板中，配置 `Default Pawn Class`、`Player Controller Class`、`HUD Class` 等。

#### 3. 设置GameMode为默认

将新的GameMode类设置为当前关卡或项目的默认GameMode：

1. 打开Unreal Engine编辑器，选择 `Edit` > `Project Settings`。
2. 在项目设置中，导航到 `Maps & Modes`。
3. 在 `Default Modes` 下，将 `GameMode` 设置为你新创建的 `MyGameMode`。
4. 如果你只想在特定关卡中使用这个GameMode，可以在关卡设置中进行配置：打开关卡，选择 `Edit` > `World Settings`，在 `GameMode Override` 中选择 `MyGameMode`。

### 示例：创建和配置一个简单的GameMode

#### MyGameMode.h

```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "MyGameMode.generated.h"

UCLASS()
class MYPROJECT_API AMyGameMode : public AGameModeBase
{
    GENERATED_BODY()

public:
    AMyGameMode();
};
```

#### MyGameMode.cpp

```c++
#include "MyGameMode.h"
#include "MyPlayerController.h"
#include "MyPawn.h"
#include "MyHUD.h"
#include "MyPlayerState.h"
#include "MyGameState.h"

AMyGameMode::AMyGameMode()
{
    // Set default pawn class to your custom pawn class
    DefaultPawnClass = AMyPawn::StaticClass();

// Use your custom player controller class
PlayerControllerClass = AMyPlayerController::StaticClass();

// Use your custom HUD class
HUDClass = AMyHUD::StaticClass();

// Use your custom player state class
PlayerStateClass = AMyPlayerState::StaticClass();

// Use your custom game state class
GameStateClass = AMyGameState::StaticClass();

}
```

### 关键点总结

- **游戏规则**：`GameMode` 决定了游戏的核心规则和逻辑。
- **玩家行为**：`GameMode` 管理玩家的生成、角色、控制器等。
- **默认类**：通过指定各种默认类，`GameMode` 控制游戏中不同元素的行为。
- **游戏状态**：`GameMode` 与 `GameState` 和 `PlayerState` 协同工作，管理整个游戏和单个玩家的状态。

## 类名前缀

在Unreal Engine（UE）的C++代码中，类名前缀`A`代表的是“Actor”类。这是Unreal Engine的一种命名约定，用于区分不同类型的类。以下是一些常见的前缀及其含义：

### 常见前缀及其含义

1. **A**：表示Actor类。这类对象通常是游戏世界中的实体，可以拥有位置、旋转和缩放等属性，并且可以被玩家或AI控制。
2. **U**：表示Unreal Engine的对象（Object）类。这些类通常是没有位置的逻辑对象，常用于管理数据和逻辑。
3. **F**：表示结构体（Struct）。这些类型通常用于定义数据结构和简单的数据类型。
4. **S**：表示单例（Singleton）类。这些类通常用于全局管理和单例模式。
5. **I**：表示接口（Interface）。这些类型用于定义接口类，用于实现多态和接口编程。
6. **T**：通常用于模板类（Template）。这些类使用C++模板来实现通用代码。

### 示例

#### Actor 类（前缀A）

```c++
// MyActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
    GENERATED_BODY()

public:	
    AMyActor();

protected:
    virtual void BeginPlay() override;

public:	
    virtual void Tick(float DeltaTime) override;
};
```

#### Object 类（前缀U）

```c++
// MyObject.h
#pragma once

#include "CoreMinimal.h"
#include "UObject/NoExportTypes.h"
#include "MyObject.generated.h"

UCLASS()
class MYPROJECT_API UMyObject : public UObject
{
    GENERATED_BODY()

public:
    UMyObject();
};
```

#### 结构体（前缀F）

```c++
// MyStruct.h
#pragma once

#include "CoreMinimal.h"
#include "MyStruct.generated.h"

USTRUCT(BlueprintType)
struct MYPROJECT_API FMyStruct
{
    GENERATED_BODY()

public:
    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    int32 MyInt;

UPROPERTY(EditAnywhere, BlueprintReadWrite)
float MyFloat;

};
```

#### 接口（前缀I）

```c++
// MyInterface.h
#pragma once

#include "CoreMinimal.h"
#include "UObject/Interface.h"
#include "MyInterface.generated.h"

UINTERFACE(MinimalAPI)
class UMyInterface : public UInterface
{
    GENERATED_BODY()
};

class MYPROJECT_API IMyInterface
{
    GENERATED_BODY()

public:
    virtual void MyFunction() = 0;
};
```



### 总结

Unreal Engine 使用特定的前缀来表示不同类型的类，以提高代码的可读性和一致性。以下是一些常见的前缀及其含义：

- **A**：Actor类。
- **U**：Object类。
- **F**：结构体。
- **S**：单例类。
- **I**：接口类。
- **T**：模板类。

这些前缀有助于开发者快速识别类的类型和用途，从而更好地理解和维护代码。



## 委托（delegate）

在C++中，委托（delegate）是一种可以将函数或成员函数指针绑定到事件或回调机制上的技术。在Unreal Engine中，委托机制被广泛使用，以便实现事件驱动的编程风格。Unreal Engine提供了多种类型的委托，包括单播委托（Single-cast Delegate）、多播委托（Multi-cast Delegate）和动态委托（Dynamic Delegate）。

### 单播委托（Single-cast Delegate）

单播委托绑定一个函数，适用于只需一个回调函数的场景。

```c++
// 声明单播委托类型
DECLARE_DELEGATE(FSimpleDelegate);

// 声明一个委托实例
FSimpleDelegate MyDelegate;

// 定义一个函数
void MyFunction()
{
    // 函数体
}

// 绑定函数到委托
MyDelegate.BindStatic(&MyFunction);

// 调用委托（触发绑定的函数）
MyDelegate.ExecuteIfBound();
```

### 多播委托（Multi-cast Delegate）

多播委托可以绑定多个函数，适用于需要多个回调函数的场景。

```c++
// 声明多播委托类型
DECLARE_MULTICAST_DELEGATE(FMultiDelegate);

// 声明一个多播委托实例
FMultiDelegate MyMultiDelegate;

// 定义几个函数
void MyFunction1()
{
    // 函数体
}

void MyFunction2()
{
    // 函数体
}

// 绑定函数到多播委托
MyMultiDelegate.AddStatic(&MyFunction1);
MyMultiDelegate.AddStatic(&MyFunction2);

// 调用多播委托（触发所有绑定的函数）
MyMultiDelegate.Broadcast();
```

### 动态委托（Dynamic Delegate）

动态委托允许在蓝图和C++之间进行绑定，可以用于UObject派生类的成员函数，并且支持反射。

```c++
// 在UObject派生类中声明动态委托
UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
    GENERATED_BODY()

public:
    // 动态多播委托
    DECLARE_DYNAMIC_MULTICAST_DELEGATE(FDynamicDelegate);

// 声明一个动态多播委托实例
FDynamicDelegate OnMyEvent;

// 一个绑定函数
UFUNCTION()
void MyDynamicFunction()
{
    // 函数体
}

};

// 在代码中绑定和调用动态委托
AMyActor* MyActor = NewObject<AMyActor>();
MyActor->OnMyEvent.AddDynamic(MyActor, &AMyActor::MyDynamicFunction);

// 触发委托
MyActor->OnMyEvent.Broadcast();
```

### 示例

假设你有一个事件系统，你希望在某个事件发生时通知多个监听者（函数）。

```c++
// 事件管理器类
class EventManager
{
public:
    // 声明多播委托类型
    DECLARE_MULTICAST_DELEGATE(FOnEvent);

    // 事件委托实例
    FOnEvent OnEvent;

    // 触发事件的方法
    void TriggerEvent()
    {
        OnEvent.Broadcast();
    }

};

// 监听者类
class Listener
{
    public:
        void RespondToEvent()
        {
            // 响应事件
        }
};

int main()
{
    EventManager MyEventManager;
    Listener MyListener;

// 绑定监听者的方法到事件
MyEventManager.OnEvent.AddRaw(&MyListener, &Listener::RespondToEvent);

// 触发事件
MyEventManager.TriggerEvent();

return 0;

}
```

在这个示例中，当事件管理器触发事件时，监听者的响应方法将被调用。

### 总结

Unreal Engine中的委托机制提供了强大的事件处理能力，使得代码更加模块化和易于维护。单播委托适用于单一回调的场景，多播委托适用于多个回调的场景，而动态委托则提供了在C++和蓝图之间进行交互的能力。

## 数组

在 C++ 中，标准库提供了两种常见的数组类：`std::array` 和 `std::vector`。此外，您还可以创建自己的数组类以满足特定需求。

### std::array

`std::array` 是 C++ 标准库中提供的一个静态数组类，它的大小在编译时是固定的。与普通的 C 风格数组相比，`std::array` 提供了更好的接口和更多的功能。

```c++
#include <iostream>
#include <array>

int main() {
    // 声明并初始化 std::array
    std::array<int, 5> myArray = {1, 2, 3, 4, 5};

// 访问元素
std::cout << "First element: " << myArray[0] << std::endl;

// 修改元素
myArray[1] = 10;

// 遍历数组
for (size_t i = 0; i < myArray.size(); ++i) {
    std::cout << myArray[i] << " ";
}
std::cout << std::endl;

return 0;

}
```

### std::vector

`std::vector` 是 C++ 标准库中提供的一个动态数组类，它的大小可以在运行时动态调整。`std::vector` 提供了丰富的接口，适用于需要动态调整大小的场景。

```c++
#include <iostream>
#include <vector>

int main() {
    // 声明并初始化 std::vector
    std::vector<int> myVector = {1, 2, 3, 4, 5};

// 访问元素
std::cout << "First element: " << myVector[0] << std::endl;

// 修改元素
myVector[1] = 10;

// 添加元素
myVector.push_back(6);

// 遍历数组
for (size_t i = 0; i < myVector.size(); ++i) {
    std::cout << myVector[i] << " ";
}
std::cout << std::endl;

// 删除最后一个元素
myVector.pop_back();

return 0;

}
```

### 自定义数组类

```c++
#include <iostream>
#include <stdexcept>

template <typename T>
class CustomArray {
private:
    T* data;
    size_t size;

public:
    // 构造函数
    CustomArray(size_t size) : size(size) {
        data = new T[size];
    }

// 析构函数
~CustomArray() {
    delete[] data;
}

// 重载下标运算符
T& operator[](size_t index) {
    if (index >= size) {
        throw std::out_of_range("Index out of range");
    }
    return data[index];
}

// 获取数组大小
size_t getSize() const {
    return size;
}

};

int main() {
    // 创建一个包含 5 个元素的 CustomArray
    CustomArray<int> myArray(5);

// 设置元素
myArray[0] = 1;
myArray[1] = 2;
myArray[2] = 3;

// 获取并打印元素
for (size_t i = 0; i < myArray.getSize(); ++i) {
    std::cout << myArray[i] << " ";
}
std::cout << std::endl;

return 0;

}
```

## 枚举

### 核心系统枚举

**EWorldType**

- 描述：用于表示世界的类型（游戏、编辑器等）。
- 值：
  - `EWorldType::None`
  - `EWorldType::Game`
  - `EWorldType::Editor`
  - `EWorldType::PIE`（Play In Editor）
  - `EWorldType::EditorPreview`
  - `EWorldType::GamePreview`
  - `EWorldType::GameRPC`
  - `EWorldType::Inactive`

**EComponentMobility**

- **描述**：用于表示组件的移动性。
- 值：
  - `EComponentMobility::Static`
  - `EComponentMobility::Stationary`
  - `EComponentMobility::Movable`

**ECollisionChannel**

- **描述**：用于表示碰撞通道。
- 值：
  - `ECC_WorldStatic`
  - `ECC_WorldDynamic`
  - `ECC_Pawn`
  - `ECC_Visibility`
  - `ECC_Camera`
  - `ECC_PhysicsBody`
  - `ECC_Vehicle`
  - `ECC_Destructible`
  - 其他自定义通道

### 渲染系统枚举

**EBlendMode**

- **描述**：用于表示材质的混合模式。
- 值：
  - `BLEND_Opaque`
  - `BLEND_Masked`
  - `BLEND_Translucent`
  - `BLEND_Additive`
  - `BLEND_Modulate`
  - `BLEND_AlphaComposite`
  - `BLEND_AlphaHoldout`

**ETextureCompressionSettings**

- **描述**：用于表示纹理的压缩设置。
- 值：
  - `TC_Default`
  - `TC_Normalmap`
  - `TC_Masks`
  - `TC_Grayscale`
  - `TC_Displacementmap`
  - `TC_VectorDisplacementmap`
  - `TC_HDR`
  - `TC_EditorIcon`
  - `TC_Alpha`
  - `TC_DistanceFieldFont`
  - `TC_HDR_Compressed`
  - `TC_BC7`
  - 其他设置

**EDepthOfFieldMethod**

- **描述**：用于表示景深方法。
- 值：
  - `DOFM_BokehDOF`
  - `DOFM_Gaussian`
  - `DOFM_CircleDOF`

### 动画系统枚举

**ERootMotionMode**

- **描述**：用于表示根运动模式。
- 值：
  - `ERootMotionMode::NoRootMotionExtraction`
  - `ERootMotionMode::IgnoreRootMotion`
  - `ERootMotionMode::RootMotionFromEverything`
  - `ERootMotionMode::RootMotionFromMontagesOnly`

**EAnimationMode**

- **描述**：用于表示动画模式。
- 值：
  - `EAnimationMode::AnimationBlueprint`
  - `EAnimationMode::AnimationSingleNode`
  - `EAnimationMode::AnimationCustomMode`

### 输入系统枚举

**EInputEvent**

- **描述**：用于表示输入事件类型。
- 值：
  - `IE_Pressed`
  - `IE_Released`
  - `IE_Repeat`
  - `IE_DoubleClick`
  - `IE_Axis`

**ETouchIndex**

- **描述**：用于表示触摸输入的手指索引。
- 值：
  - `ETouchIndex::Touch1`
  - `ETouchIndex::Touch2`
  - `ETouchIndex::Touch3`
  - `ETouchIndex::Touch4`
  - `ETouchIndex::Touch5`
  - `ETouchIndex::Touch6`
  - `ETouchIndex::Touch7`
  - `ETouchIndex::Touch8`
  - `ETouchIndex::Touch9`
  - `ETouchIndex::Touch10`

### AI系统枚举

**EPathFollowingRequestResult**

- **描述**：用于表示路径跟随请求的结果。
- 值：
  - `EPathFollowingRequestResult::Failed`
  - `EPathFollowingRequestResult::AlreadyAtGoal`
  - `EPathFollowingRequestResult::RequestSuccessful`

**EAIRequestPriority**

- **描述**：用于表示AI请求的优先级。
- 值：
  - `EAIRequestPriority::SoftScript`
  - `EAIRequestPriority::Logic`
  - `EAIRequestPriority::HardScript`
  - `EAIRequestPriority::Reaction`
  - `EAIRequestPriority::Ultimate`

## HUD（Heads-Up Display）

在Unreal Engine中，`HUD` 类（Heads-Up Display）用于绘制和管理游戏中的用户界面元素，例如玩家的生命值、得分、计时器等。`HUD` 类允许开发者在屏幕上绘制2D元素和文本，并与玩家交互。以下是关于`HUD` 类的详细介绍和使用示例。

### 创建和使用HUD类

#### 1. 创建HUD类

首先，在你的Unreal Engine项目中创建一个新的HUD类。

1. **创建C++ HUD类**：
   - 打开Unreal Engine编辑器。
   - 在内容浏览器中，右键点击空白区域，选择 `Create` > `C++ Class`。
   - 选择 `HUD` 作为父类，然后点击 `Next`。
   - 命名你的HUD类，例如 `MyHUD`，然后点击 `Create Class`。
2. **创建Blueprint HUD类**（可选）：
   - 你可以基于C++类创建一个蓝图类，以便在蓝图中进一步扩展和定制HUD。

#### 2. 实现HUD类

在你的HUD类中，重写 `DrawHUD` 方法以实现自定义绘制逻辑。

##### MyHUD.h

```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/HUD.h"
#include "MyHUD.generated.h"

UCLASS()
class MYPROJECT_API AMyHUD : public AHUD
{
    GENERATED_BODY()

public:
    virtual void DrawHUD() override;

private:
    void DrawCrosshair();
    void DrawHealthBar();
};
```

##### MyHUD.cpp

```c++
#include "MyHUD.h"
#include "Engine/Canvas.h"
#include "Engine/Texture2D.h"
#include "UObject/ConstructorHelpers.h"

AMyHUD::AMyHUD()
{
    // Constructor logic, e.g., loading textures
}

void AMyHUD::DrawHUD()
{
    Super::DrawHUD();

// Custom drawing logic
DrawCrosshair();
DrawHealthBar();

}

void AMyHUD::DrawCrosshair()
{
    // Example logic to draw a crosshair in the center of the screen
    FVector2D Center(Canvas->ClipX * 0.5f, Canvas->ClipY * 0.5f);
    FVector2D CrosshairDrawPosition(Center.X - 8.0f, Center.Y - 8.0f);

// Assume CrosshairTex is a UTexture2D* loaded in the constructor
FCanvasTileItem TileItem(CrosshairDrawPosition, CrosshairTex->Resource, FLinearColor::White);
TileItem.BlendMode = SE_BLEND_Translucent;
Canvas->DrawItem(TileItem);

}

void AMyHUD::DrawHealthBar()
{
    // Example logic to draw a health bar
    float Health = 75.0f; // Placeholder health value
    FVector2D HealthBarPosition(50.0f, 50.0f);
    FVector2D HealthBarSize(200.0f, 25.0f);

// Draw health bar background
FCanvasBoxItem BackgroundBox(HealthBarPosition, HealthBarSize);
BackgroundBox.SetColor(FLinearColor::Black);
Canvas->DrawItem(BackgroundBox);

// Draw health bar fill
FVector2D HealthBarFillSize(HealthBarSize.X * (Health / 100.0f), HealthBarSize.Y);
FCanvasBoxItem HealthBox(HealthBarPosition, HealthBarFillSize);
HealthBox.SetColor(FLinearColor::Green);
Canvas->DrawItem(HealthBox);

}
```

#### 3.设置自定义HUD类

将自定义的HUD类设置为游戏的默认HUD类。

##### 1.**在GameMode中设置HUD类**

- 打开你的GameMode类，确保在`GameMode`类中使用自定义的HUD类。

###### MyGameMode.h

```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "MyGameMode.generated.h"

UCLASS()
class MYPROJECT_API AMyGameMode : public AGameModeBase
{
    GENERATED_BODY()

public:
    AMyGameMode();
};
```

###### MyGameMode.cpp

```c++
#include "MyGameMode.h"
#include "MyHUD.h"

AMyGameMode::AMyGameMode()
{
    // Set default pawn class to your custom pawn class
    DefaultPawnClass = AMyPawn::StaticClass();

// Set custom HUD class
HUDClass = AMyHUD::StaticClass();

}
```

##### 2.**在项目设置中设置默认GameMode**

- 打开项目设置，导航到 `Maps & Modes`，并确保默认GameMode设置为你的自定义GameMode。

### 使用Blueprint扩展HUD

你也可以使用Blueprint进一步扩展和定制HUD。

1. ##### **创建基于C++ HUD类的Blueprint类**：

   - 在内容浏览器中，右键点击你的C++ HUD类（例如，`MyHUD`），选择 `Create Blueprint Class based on MyHUD`。
   - 打开创建的Blueprint类，可以在其中添加和配置Widget、动画等。

2. #### **在Blueprint中实现绘制逻辑**：

   - 使用UMG（Unreal Motion Graphics）在Blueprint中创建和管理UI元素，如生命值、得分、计时器等。

### 总结

`HUD` 类在Unreal Engine中用于管理和绘制游戏的用户界面元素。通过创建和配置自定义HUD类，你可以在屏幕上绘制各种2D元素和文本，并与玩家交互。结合C++和Blueprint，你可以创建功能强大且美观的游戏UI。

## FVector

`FVector` 是 Unreal Engine 中用于表示三维空间中点或方向的基本数据类型。它主要包含三个浮点数，分别表示 X、Y 和 Z 轴的坐标或方向分量。

### FVector 详解

#### 1.FVector 结构

`FVector` 的结构如下：

```c++
struct FVector
{
    float X;
    float Y;
    float Z;
    

// Constructors
FVector();
FVector(float InF);
FVector(float InX, float InY, float InZ);

// Operator Overloads
FVector operator+(const FVector& V) const;
FVector operator-(const FVector& V) const;
FVector operator*(float Scale) const;
FVector operator/(float Scale) const;
bool operator==(const FVector& V) const;
bool operator!=(const FVector& V) const;

// Other functions
float Size() const;
float SizeSquared() const;
FVector GetSafeNormal(float Tolerance = SMALL_NUMBER) const;
FVector CrossProduct(const FVector& A, const FVector& B);
float DotProduct(const FVector& A, const FVector& B);

};
```

#### 2.构造函数

`FVector` 有多个构造函数，可以用不同的方式初始化：

```c++
FVector A;                    // 默认构造函数，初始化为 (0,0,0)
FVector B(1.0f);              // 所有分量初始化为 1.0
FVector C(1.0f, 2.0f, 3.0f);  // 初始化为 (1.0, 2.0, 3.0)
```

#### 3.操作符重载

`FVector` 支持多种操作符重载：

加法运算：

```c++
FVector D = A + B;
```

减法运算：

```c++
FVector E = C - B;
```

乘法运算：

```c++
FVector F = C * 2.0f;
```

除法运算：

```c++
FVector G = C / 2.0f;
```

相等和不相等判断：

```c++
bool IsEqual = (A == B);
bool IsNotEqual = (A != B);
```

#### 4.常用函数

`FVector` 提供了一些常用的函数，用于计算向量的大小、标准化、点乘和叉乘等。

向量的大小：

```c++
float Size = C.Size();
float SizeSquared = C.SizeSquared();
```

向量的标准化：

```c++
FVector NormalizedVector = C.GetSafeNormal();
```

点乘和叉乘：

```c++
float Dot = FVector::DotProduct(A, B);
FVector Cross = FVector::CrossProduct(A, B);
```

### 示例代码

```c++
void Example()
{
    FVector VectorA(1.0f, 2.0f, 3.0f);
    FVector VectorB(4.0f, 5.0f, 6.0f);
    

// 向量加法
FVector ResultAdd = VectorA + VectorB;

// 向量减法
FVector ResultSubtract = VectorA - VectorB;

// 向量乘以标量
FVector ResultMultiply = VectorA * 2.0f;

// 向量除以标量
FVector ResultDivide = VectorA / 2.0f;

// 向量大小
float Size = VectorA.Size();

// 向量标准化
FVector NormalizedVector = VectorA.GetSafeNormal();

// 点乘和叉乘
float DotProduct = FVector::DotProduct(VectorA, VectorB);
FVector CrossProduct = FVector::CrossProduct(VectorA, VectorB);

}
```

## FRotator

`FRotator` 是 Unreal Engine 中用于表示旋转的一个类。它使用三个浮点数来表示旋转的三个轴，即 Pitch、Yaw 和 Roll。这三个轴的定义如下：

Pitch 绕 X 轴旋转，通常表示上下视角的旋转。

Yaw 绕 Z 轴旋转，通常表示左右视角的旋转。

Roll 绕 Y 轴旋转，通常表示旋转对象的翻滚。

### `FRotator` 的定义

以下是 `FRotator` 的定义：

```c++
struct FRotator
{
    float Pitch;
    float Yaw;
    float Roll;

// Constructors
FRotator() : Pitch(0), Yaw(0), Roll(0) {}
FRotator(float InPitch, float InYaw, float InRoll) : Pitch(InPitch), Yaw(InYaw), Roll(InRoll) {}

// Functions for converting to other formats, normalization, etc.
FQuat Quaternion() const;
FVector Vector() const;
void Normalize();
FRotator GetNormalized() const;
FRotator GetInverse() const;
bool IsNearlyZero(float Tolerance = KINDA_SMALL_NUMBER) const;
bool Equals(const FRotator& R, float Tolerance = KINDA_SMALL_NUMBER) const;

// Overloaded operators for arithmetic and comparisons
FRotator operator+(const FRotator& R) const;
FRotator operator-(const FRotator& R) const;
FRotator operator*(float Scale) const;
FRotator operator/(float Scale) const;
bool operator==(const FRotator& R) const;
bool operator!=(const FRotator& R) const;

};
```

### 主要函数

#### 构造函数

- `FRotator()`：默认构造函数，将 Pitch, Yaw 和 Roll 初始化为 0。
- `FRotator(float InPitch, float InYaw, float InRoll)`：使用指定的 Pitch, Yaw 和 Roll 初始化。

#### 四元数转换

- `FQuat Quaternion() const`：将 `FRotator` 转换为四元数（`FQuat`）。

#### 向量转换

- `FVector Vector() const`：将 `FRotator` 转换为方向向量（`FVector`）。

#### 规范化

- `void Normalize()`：规范化 `FRotator`，确保角度在有效范围内。
- `FRotator GetNormalized() const`：返回规范化的 `FRotator`。

#### 求逆

- `FRotator GetInverse() const`：返回旋转的逆。

#### 接近零的检查

- `bool IsNearlyZero(float Tolerance = KINDA_SMALL_NUMBER) const`：检查旋转是否接近零。

#### 相等比较

- `bool Equals(const FRotator& R, float Tolerance = KINDA_SMALL_NUMBER) const`：检查两个 `FRotator` 是否在指定公差内相等。

#### 重载运算符

- `FRotator operator+(const FRotator& R) const`：旋转相加。
- `FRotator operator-(const FRotator& R) const`：旋转相减。
- `FRotator operator*(float Scale) const`：旋转缩放。
- `FRotator operator/(float Scale) const`：旋转除法。
- `bool operator==(const FRotator& R) const`：检查旋转是否相等。
- `bool operator!=(const FRotator& R) const`：检查旋转是否不相等。

### 使用示例

以下是一些 `FRotator` 的使用示例：

```c++
// 创建一个 FRotator
FRotator MyRotator(30.0f, 45.0f, 60.0f);

// 将 FRotator 转换为四元数
FQuat MyQuat = MyRotator.Quaternion();

// 将 FRotator 转换为方向向量
FVector MyVector = MyRotator.Vector();

// 规范化旋转
MyRotator.Normalize();

// 检查旋转是否接近零
bool bIsNearlyZero = MyRotator.IsNearlyZero();

// 旋转相加
FRotator RotatorA(10.0f, 20.0f, 30.0f);
FRotator RotatorB(5.0f, 15.0f, 25.0f);
FRotator Result = RotatorA + RotatorB;

// 比较旋转是否相等
bool bEqual = (RotatorA == RotatorB);
```

## FTransform

`FTransform`是Unreal Engine 4 (UE4)中的一个类，用于表示对象在世界空间中的位置、旋转和缩放。`FTransform`类提供了许多用于操作这些变换的函数。它包含了位置（Translation）、旋转（Rotation）和缩放（Scale），并且可以用来描述对象在三维空间中的完整变换。

### 主要成员变量

Translation(位置)：表示对象的位置。

Rotation(旋转)：表示对象的旋转，通常使用四元数（`FQuat`）表示。

Scale(缩放)：表示对象的缩放。

### 主要方法

#### 构造函数

- `FTransform()`：默认构造函数，创建一个单位变换（无变换）。
- `FTransform(const FQuat& InRotation)`: 使用旋转初始化。
- `FTransform(const FVector& InTranslation)`: 使用位置初始化。
- `FTransform(const FVector& InTranslation, const FQuat& InRotation, const FVector& InScale3D)`: 使用位置、旋转和缩放初始化。

#### 基本方法

- `void SetLocation(const FVector& NewLocation)`: 设置位置。
- `void SetRotation(const FQuat& NewRotation)`: 设置旋转。
- `void SetScale3D(const FVector& NewScale3D)`: 设置缩放。
- `FVector GetLocation() const`: 获取位置。
- `FQuat GetRotation() const`: 获取旋转。
- `FVector GetScale3D() const`: 获取缩放。

#### 组合和逆变换

- `FTransform GetRelativeTransform(const FTransform& Other) const`: 获取相对变换。
- `FTransform GetInverse() const`: 获取当前变换的逆变换。

#### 转换

- `FVector TransformPosition(const FVector& V) const`: 转换位置向量。
- `FVector TransformVector(const FVector& V) const`: 转换方向向量。

#### 插值

- `FTransform Blend(const FTransform& Atom1, const FTransform& Atom2, float Alpha)`: 线性插值两个变换。
- `FTransform BlendWith(const FTransform& OtherAtom, float Alpha)`: 当前变换与另一个变换进行线性插值。

### 示例代码

```c++
#include "GameFramework/Actor.h"
#include "Math/Transform.h"

void Example()
{
    // 创建一个初始位置为(0, 0, 0)的变换
    FTransform Transform1(FVector(0.0f, 0.0f, 0.0f));
    

// 设置旋转
FQuat Rotation = FQuat::MakeFromEuler(FVector(0.0f, 90.0f, 0.0f));
Transform1.SetRotation(Rotation);

// 设置缩放
Transform1.SetScale3D(FVector(2.0f, 2.0f, 2.0f));

// 获取位置
FVector Location = Transform1.GetLocation();

// 获取逆变换
FTransform InverseTransform = Transform1.GetInverse();

// 转换位置
FVector TransformedPosition = Transform1.TransformPosition(FVector(1.0f, 1.0f, 1.0f));

}
```

## FMatrix

### FMatrix 类简介

`FMatrix` 类定义在 `Math/UnrealMath.h` 中，它是一个 4x4 的矩阵，常用于变换点、向量和其他矩阵运算。`FMatrix` 在三维图形学中非常有用，尤其是在处理视图变换、投影变换和模型变换时。

### FMatrix 的基本结构

一个 `FMatrix` 实例包含 4 行，每行包含 4 个元素：

```c++
class FMatrix
{
public:
    FVector4 M[4];

// 构造函数、运算符重载、成员函数等...

};
```

### 常见用法

#### 构造函数

```c++
FMatrix::FMatrix(const FVector4& InX, const FVector4& InY, const FVector4& InZ, const FVector4& InW);
```

#### 常用方法和操作

**Identity 矩阵**：生成一个单位矩阵。

```c++
FMatrix IdentityMatrix = FMatrix::Identity;
```

**矩阵乘法**：可以将两个矩阵相乘。

```c++
FMatrix Result = MatrixA * MatrixB;
```

**转置矩阵**：返回当前矩阵的转置。

```c++
FMatrix TransposedMatrix = Matrix.Transpose();
```

**矩阵逆**：返回当前矩阵的逆。

```c++
FMatrix InverseMatrix = Matrix.Inverse();
```

**转换向量**：将一个 `FVector4` 向量乘以矩阵，得到变换后的向量。

```c++
FVector4 TransformedVector = Matrix.TransformVector4(Vector);
```

### 示例代码

```c++
#include "CoreMinimal.h"
#include "Math/UnrealMathUtility.h"
#include "Math/Vector4.h"
#include "Math/Matrix.h"

void MatrixExample()
{
    // 定义行向量
    FVector4 Row0(1, 0, 0, 0);
    FVector4 Row1(0, 1, 0, 0);
    FVector4 Row2(0, 0, 1, 0);
    FVector4 Row3(0, 0, 0, 1);

// 创建一个单位矩阵
FMatrix IdentityMatrix(Row0, Row1, Row2, Row3);

// 打印矩阵
UE_LOG(LogTemp, Warning, TEXT("Identity Matrix:\n%s"), *IdentityMatrix.ToString());

// 定义另一个矩阵
FMatrix AnotherMatrix(
    FVector4(2, 0, 0, 0),
    FVector4(0, 2, 0, 0),
    FVector4(0, 0, 2, 0),
    FVector4(0, 0, 0, 2)
);

// 矩阵乘法
FMatrix ResultMatrix = IdentityMatrix * AnotherMatrix;
UE_LOG(LogTemp, Warning, TEXT("Result Matrix:\n%s"), *ResultMatrix.ToString());

// 转置矩阵
FMatrix TransposedMatrix = AnotherMatrix.Transpose();
UE_LOG(LogTemp, Warning, TEXT("Transposed Matrix:\n%s"), *TransposedMatrix.ToString());

// 矩阵逆
FMatrix InverseMatrix = AnotherMatrix.Inverse();
UE_LOG(LogTemp, Warning, TEXT("Inverse Matrix:\n%s"), *InverseMatrix.ToString());

// 变换向量
FVector4 Vector(1, 2, 3, 1);
FVector4 TransformedVector = AnotherMatrix.TransformVector4(Vector);
UE_LOG(LogTemp, Warning, TEXT("Transformed Vector: %s"), *TransformedVector.ToString());

}
```

## FQuat

`FQuat` 是 Unreal Engine 4 (UE4) 中的四元数类，用于表示和操作三维空间中的旋转。四元数是一种数学表示方法，避免了欧拉角表示法中常见的万向节锁（Gimbal Lock）问题，并且计算高效，适合用于计算机图形学中的旋转表示。

### 主要成员变量

`FQuat` 类包含四个成员变量：

`X`：四元数的 x 分量

`Y`：四元数的 y 分量

`Z`：四元数的 z 分量

`W`：四元数的 w 分量

### 主要方法

#### 构造函数

- `FQuat()`：默认构造函数，创建一个单位四元数。
- `FQuat(float InX, float InY, float InZ, float InW)`：用指定的 x, y, z, w 分量构造四元数。
- `FQuat(const FRotator& R)`：用欧拉角（FRotator）构造四元数。

#### 基本方法

- `void Normalize(float Tolerance=SMALL_NUMBER)`：将四元数归一化。
- `FQuat Inverse() const`：计算四元数的逆。
- `float Size() const`：计算四元数的大小。
- `float SizeSquared() const`：计算四元数的大小的平方。
- `bool IsNormalized() const`：检查四元数是否已归一化。

#### 插值

- `static FQuat Slerp(const FQuat& Quat1, const FQuat& Quat2, float Slerp)`：使用球形线性插值（Slerp）在两个四元数之间插值。
- `static FQuat FastLerp(const FQuat& A, const FQuat& B, float Alpha)`：快速线性插值。
- `static FQuat FastBilerp(const FQuat& P00, const FQuat& P10, const FQuat& P01, const FQuat& P11, float FracX, float FracY)`：双线性插值。

#### 转换

- `FVector RotateVector(const FVector& V) const`：用四元数旋转向量。
- `FVector UnrotateVector(const FVector& V) const`：用四元数的逆旋转向量。
- `FRotator Rotator() const`：将四元数转换为欧拉角（FRotator）。

### 示例代码

```c++
#include "Math/Quat.h"
#include "Math/Rotator.h"
#include "Math/Vector.h"

void Example()
{
    // 创建一个默认的单位四元数
    FQuat Quat1;
    

// 使用指定的 x, y, z, w 分量创建四元数
FQuat Quat2(0.0f, 0.707f, 0.0f, 0.707f);

// 使用欧拉角创建四元数
FRotator Rotator(90.0f, 0.0f, 0.0f);
FQuat Quat3(Rotator);

// 归一化四元数
Quat2.Normalize();

// 计算四元数的逆
FQuat InverseQuat = Quat2.Inverse();

// 计算插值
FQuat SlerpedQuat = FQuat::Slerp(Quat1, Quat2, 0.5f);

// 用四元数旋转向量
FVector Vector(1.0f, 0.0f, 0.0f);
FVector RotatedVector = Quat2.RotateVector(Vector);

// 将四元数转换为欧拉角
FRotator NewRotator = Quat2.Rotator();

}
```

### 四元数旋转的优势

1. **避免万向节锁**：使用欧拉角表示旋转时，某些角度组合会导致万向节锁，无法表达任意方向的旋转。四元数通过数学性质避免了这个问题。
2. **插值平滑**：四元数插值（如 Slerp）在动画和物理计算中提供了更平滑的旋转效果。
3. **计算效率**：四元数乘法和归一化操作通常比欧拉角转换矩阵更高效。



## FColor

在 Unreal Engine 中，`FColor` 是一个用于表示颜色的结构体，包含红、绿、蓝和透明度（Alpha）四个分量。每个分量都是一个 8 位无符号整数（`uint8`），范围从 0 到 255。`FColor` 类通常用于表示非线性颜色值，适合直接在屏幕上显示的颜色。

### `FColor` 结构体

以下是 `FColor` 结构体的定义及其常用成员函数和静态成员变量。

```c++
struct FColor
{
    // Color components in 8-bit unsigned integers
    uint8 R;
    uint8 G;
    uint8 B;
    uint8 A;

// Constructors
FColor();
FColor(uint8 InR, uint8 InG, uint8 InB, uint8 InA = 255);

// Some common color constants
static const FColor White;
static const FColor Black;
static const FColor Red;
static const FColor Green;
static const FColor Blue;
static const FColor Yellow;
static const FColor Cyan;
static const FColor Magenta;

// Conversion to linear color
FLinearColor ReinterpretAsLinear() const;

// Static methods
static FColor MakeRandomColor();
static FColor MakeRedToGreenColorFromScalar(float Scalar);

};
```

### 构造函数

`FColor` 有多个构造函数，允许不同方式初始化颜色值：

默认构造函数初始化为黑色。

带参数的构造函数允许指定红、绿、蓝和透明度（Alpha）值。

```c++
FColor::FColor()
    : R(0), G(0), B(0), A(255) {}

FColor::FColor(uint8 InR, uint8 InG, uint8 InB, uint8 InA)
    : R(InR), G(InG), B(InB), A(InA) {}
```

### 常用颜色常量

`FColor` 提供了一些预定义的常用颜色常量，例如白色、黑色、红色等。

```c++
const FColor FColor::White(255, 255, 255);
const FColor FColor::Black(0, 0, 0);
const FColor FColor::Red(255, 0, 0);
const FColor FColor::Green(0, 255, 0);
const FColor FColor::Blue(0, 0, 255);
const FColor FColor::Yellow(255, 255, 0);
const FColor FColor::Cyan(0, 255, 255);
const FColor FColor::Magenta(255, 0, 255);
```

### 颜色转换

`FColor` 可以转换为 `FLinearColor`，这是一种线性颜色表示，更适合进行颜色计算和光照计算。

```c++
FLinearColor FColor::ReinterpretAsLinear() const
{
    return FLinearColor(R / 255.0f, G / 255.0f, B / 255.0f, A / 255.0f);
}
```

### 静态方法

`FColor` 提供了一些静态方法生成随机颜色或根据标量值生成从红到绿的渐变颜色。

```c++
FColor FColor::MakeRandomColor()
{
    return FColor(FMath::Rand() % 256, FMath::Rand() % 256, FMath::Rand() % 256);
}

FColor FColor::MakeRedToGreenColorFromScalar(float Scalar)
{
    const float ClampedScalar = FMath::Clamp(Scalar, 0.0f, 1.0f);
    return FColor(
        static_cast<uint8>((1.0f - ClampedScalar) * 255.0f),
        static_cast<uint8>(ClampedScalar * 255.0f),
        0
    );
}
```

### 使用示例

```c++
// 创建一个红色的 FColor 对象
FColor MyColor(255, 0, 0);

// 使用预定义的颜色常量
FColor GreenColor = FColor::Green;

// 生成一个随机颜色
FColor RandomColor = FColor::MakeRandomColor();

// 将 FColor 转换为 FLinearColor
FLinearColor LinearColor = MyColor.ReinterpretAsLinear();
```

`FColor` 是 Unreal Engine 中一个非常重要的类，用于处理和表示颜色，在绘制、材质和光照等方面有广泛的应用。

## FHitResult

在 Unreal Engine 中，`FHitResult` 结构体用于描述一次射线检测或碰撞检测的结果。它包含了关于碰撞的详细信息，如碰撞点、法线、撞击的物体等。`FHitResult` 常用于物理系统和射线追踪功能。

### `FHitResult` 结构体

以下是 `FHitResult` 结构体的定义及其主要成员：

```c++
struct FHitResult
{
    // Indicates if this hit was a blocking hit
    uint32 bBlockingHit:1;

// Indicates if the initial trace started penetrating
uint32 bStartPenetrating:1;

// 'Time' of impact along the trace direction
float Time;

// Distance of the hit along the trace direction
float Distance;

// Location where the hit occurred (in world space)
FVector ImpactPoint;

// Normal at the point of impact (in world space)
FVector ImpactNormal;

// Location where the hit occurred (in world space)
FVector Location;

// Normal at the point of impact (in world space)
FVector Normal;

// Start location of the trace (in world space)
FVector TraceStart;

// End location of the trace (in world space)
FVector TraceEnd;

// The actor that was hit
TWeakObjectPtr<AActor> Actor;

// The component that was hit
TWeakObjectPtr<UPrimitiveComponent> Component;

// Bone name if we hit a skeletal mesh
FName BoneName;

// Face index we hit (for complex collision)
int32 FaceIndex;

// Physical material we hit
TWeakObjectPtr<UPhysicalMaterial> PhysMaterial;

// Hit result
FHitResult()
    : bBlockingHit(false)
    , bStartPenetrating(false)
    , Time(0.0f)
    , Distance(0.0f)
    , ImpactPoint(ForceInit)
    , ImpactNormal(ForceInit)
    , Location(ForceInit)
    , Normal(ForceInit)
    , TraceStart(ForceInit)
    , TraceEnd(ForceInit)
    , Actor(nullptr)
    , Component(nullptr)
    , BoneName(NAME_None)
    , FaceIndex(INDEX_NONE)
    , PhysMaterial(nullptr)
{}

};


```

### 主要成员变量

`bBlockingHit`：是否是阻挡命中。如果是穿透命中，该值为 `false`。

`bStartPenetrating`：初始检测是否开始穿透。

`Time`：在射线方向上的命中时间（范围 0.0 到 1.0）。

`Distance`：在射线方向上的命中距离。

`ImpactPoint`：命中点的位置（世界空间）。

`ImpactNormal`：命中点的法线（世界空间）。

`Location`：命中点的位置（世界空间），与 `ImpactPoint` 类似。

`Normal`：命中点的法线（世界空间），与 `ImpactNormal` 类似。

`TraceStart`：射线的起点（世界空间）。

`TraceEnd`：射线的终点（世界空间）。

`Actor`：被命中的 Actor。

`Component`：被命中的组件。

`BoneName`：如果命中了骨骼网格，则为命中的骨骼名称。

`FaceIndex`：命中的面索引（用于复杂碰撞）。

`PhysMaterial`：命中的物理材质。

### 使用示例

以下是一个使用 `FHitResult` 的示例代码，通过射线追踪检测前方物体的命中情况：

```c++
void AMyCharacter::PerformRayTrace()
{
    FVector Start = GetActorLocation();
    FVector ForwardVector = GetActorForwardVector();
    FVector End = Start + (ForwardVector * 1000.0f);

FHitResult HitResult;
FCollisionQueryParams CollisionParams;

// 执行射线检测
bool bHit = GetWorld()->LineTraceSingleByChannel(
    HitResult,
    Start,
    End,
    ECC_Visibility,
    CollisionParams
);

// 如果命中
if (bHit)
{
    UE_LOG(LogTemp, Warning, TEXT("We hit something: %s"), *HitResult.Actor->GetName());
    UE_LOG(LogTemp, Warning, TEXT("Impact Point: %s"), *HitResult.ImpactPoint.ToString());
}

}
```

在这个例子中，`LineTraceSingleByChannel` 函数用于进行射线检测，并将结果存储在 `FHitResult` 结构体中。然后可以检查 `FHitResult` 的各个属性来获取碰撞信息，如命中的 Actor、命中点的位置等。



## FTimerHandle

`FTimerHandle` 是在 Unreal Engine 中用于处理计时器的一个类。计时器允许你在指定的时间间隔后执行函数，这在游戏编程中非常常见。以下是有关 `FTimerHandle` 的详细解释以及如何使用它的示例。

### 主要方法

```c++
GetWorldTimerManager().SetTimer(FTimerHandle& InOutHandle, ...);
```

- 设置一个计时器，并将其句柄存储在 `InOutHandle` 中。

```c++
GetWorldTimerManager().ClearTimer(const FTimerHandle& InHandle);
```

- 清除指定的计时器。

```c++
GetWorldTimerManager().IsTimerActive(const FTimerHandle& InHandle) const;
```

- 检查计时器是否处于活动状态。

### 使用示例

.h

```c++
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class MYPROJECT_API AMyActor : public AActor
{
    GENERATED_BODY()
    
public:    
    AMyActor();

protected:
    virtual void BeginPlay() override;

public:    
    virtual void Tick(float DeltaTime) override;

private:
    void MyTimerFunction();
    

FTimerHandle MyTimerHandle;

};
```

.cpp

```c++
#include "MyActor.h"
#include "Engine/World.h"
#include "TimerManager.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyActor::BeginPlay()
{
    Super::BeginPlay();
    
	// 设置计时器，在3秒后调用MyTimerFunction，每3秒重复一次
    GetWorld()->GetTimerManager().SetTimer(MyTimerHandle, 
                                           this, 
                                           &AMyActor::MyTimerFunction, 
                                           3.0f, 
                                           true);
}

void AMyActor::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);

    // 在每一帧检查计时器是否有效
    if (GetWorld()->GetTimerManager().IsTimerActive(MyTimerHandle))
    {
        // 执行一些逻辑
    }

}

void AMyActor::MyTimerFunction()
{
    // 计时器触发时执行的逻辑
    UE_LOG(LogTemp, Warning, TEXT("Timer function called"));
    

    // 例如，可以在某种条件下清除计时器
    if (/* some condition */)
    {
        GetWorld()->GetTimerManager().ClearTimer(MyTimerHandle);
    }

}
```

## FTimerDelegate 

### 1. 定义

`FTimerDelegate` 是 Unreal Engine 中的一个类型，专门用于表示定时器回调的委托。它可以绑定一个函数或方法，当定时器到期时，该函数或方法将被调用。

### 2. 使用

你可以使用 `FTimerDelegate` 来创建一个定时器，并指定定时器到期时要调用的函数。这个函数可以是全局函数、静态函数，也可以是类的成员函数。

```c++
// 包含头文件
#include "TimerManager.h"
#include "Engine/World.h"

// 定义一个类
class MyClass
{
public:
    // 定义一个成员函数，将作为定时器的回调函数
    void MyTimerFunction()
    {
        UE_LOG(LogTemp, Warning, TEXT("Timer triggered!"));
    }

// 函数，用于设置定时器
void SetMyTimer(UWorld* World)
{
    // 创建一个FTimerDelegate实例
    FTimerDelegate TimerDelegate;

​    // 绑定成员函数到委托
​    TimerDelegate.BindUObject(this, &MyClass::MyTimerFunction);

​    // 创建一个定时器句柄
​    FTimerHandle TimerHandle;

​    // 设置定时器
​    World->GetTimerManager().SetTimer(TimerHandle, TimerDelegate, 5.0f, false);
}

};

// 在某个函数中使用
void SomeFunction(UWorld* World)
{
    MyClass Instance;
    Instance.SetMyTimer(World);
}
```

## PCG

### 1. 什么是PCG？

PCG即程序化内容生成，指的是通过程序算法自动生成内容，而不是通过手工方式创建。这种方法在游戏开发中应用广泛，尤其适用于大规模场景、地图生成、道具分布等。

### 2. PCG的应用场景

地形生成：使用噪声函数和其他算法生成复杂的地形。

地图生成：自动生成随机地图或关卡布局，增加游戏的可重复性和随机性。

道具和敌人分布：根据预设规则自动分布游戏中的道具、敌人等元素。

植被和环境细节：生成树木、草地、岩石等环境细节，创建更加自然的场景。

### 3. 如何在UE中使用PCG

#### 使用Blueprints

UE中的Blueprints提供了强大的可视化脚本系统，可以用于实现PCG逻辑。例如，使用噪声函数生成地形或随机分布物体。

##### 创建Blueprint

- 在内容浏览器中，右键单击，选择“Blueprint Class”。
- 选择Actor作为基类，创建一个新的Blueprint。

##### 添加PCG逻辑

- 打开创建的Blueprint，添加一个Construction Script。
- 使用节点如“Perlin Noise”、“For Loop”、“Add Instance”来实现PCG逻辑。

#### 使用C++代码

如果需要更高的性能或复杂的逻辑，可以使用C++来实现PCG。

##### 创建C++类

- 在内容浏览器中，右键单击，选择“New C++ Class”。
- 选择Actor作为基类，创建一个新的C++类。

##### 编写PCG逻辑

- 在创建的C++类中，重写OnConstruction方法或Tick方法，添加PCG逻辑。

```c++
void AMyProceduralActor::OnConstruction(const FTransform& Transform)
{
    Super::OnConstruction(Transform);

// 示例：生成简单的网格
for (int32 i = 0; i < 100; ++i)
{
    FVector Location = FVector(FMath::RandRange(-500, 500), FMath::RandRange(-500, 500), 0);
    FRotator Rotation = FRotator::ZeroRotator;
    FActorSpawnParameters SpawnParams;

​    // 在指定位置生成静态网格体
​    GetWorld()->SpawnActor<AStaticMeshActor>(AStaticMeshActor::StaticClass(), Location, Rotation, SpawnParams);
}

}
```

#### 使用第三方插件

UE市场上有许多强大的PCG插件，如Dungeon Architect、Procedural Landscape Ecosystem等，可以大大简化PCG的实现过程。

### 4. 常用的PCG算法

​	噪声函数：如Perlin Noise、Simplex Noise，用于生成高度图、地形等。

​	随机数生成：用于生成随机位置、旋转、缩放等属性。

​	L系统（Lindenmayer System）：用于生成植被等分形图形。

​	细胞自动机：用于生成复杂的图案或地形，如洞穴系统。

### 5. PCG的优缺点

#### 优点

- 提高效率：减少手工创建内容的工作量。
- 增加多样性：生成更多样化的内容，提升游戏的可重复性。
- 实时生成：在游戏运行时动态生成内容，增强游戏的随机性和不可预测性。

#### 缺点

- 控制性差：生成内容的可控性和精细度不如手工创建。
- 调试困难：复杂的生成逻辑可能导致调试困难。
- 性能开销：实时生成复杂内容可能带来性能开销。

通过合理使用PCG，开发者可以大大提高开发效率，创造丰富多样的游戏内容。如果你有具体的PCG需求或问题，可以进一步详细描述，我可以为你提供更具体的指导。

## UMG（Unreal Motion Graphics）

UMG（Unreal Motion Graphics）是虚幻引擎（Unreal Engine）中用于创建用户界面（UI）的工具。它提供了强大的功能，可以帮助开发者快速创建和管理游戏中的UI元素，如按钮、文本框、图像等

### 关键概念

1. **Widget**：UMG中的每个UI元素都称为一个Widget。Widget可以是按钮、文本、图像、进度条等。
2. **Widget Blueprint**：这是用来设计和实现UI的主要工具。通过Widget Blueprint，开发者可以可视化地拖放Widget，并为它们添加行为。
3. **Widget Hierarchy**：Widget可以嵌套在一起，形成一个层次结构。例如，一个按钮可以包含一个文本Widget。
4. **Bindings**：UMG允许绑定数据到Widget，以便动态更新UI。例如，可以绑定玩家的分数到一个文本Widget，实时显示分数变化。

### 创建UMG Widget

#### 1.**创建Widget Blueprint**

- 右键单击内容浏览器，选择`User Interface`，然后选择`Widget Blueprint`。
- 为新的Widget Blueprint命名并打开它。

#### 2.**设计UI**

- 在Widget Blueprint编辑器中，可以看到画布（Canvas Panel）和工具面板。
- 从工具面板中拖放不同的Widget到画布上，如按钮（Button）、文本（Text Block）、图像（Image）等。
- 可以调整每个Widget的属性，如位置、大小、颜色等。

#### 3.**添加行为**

在事件图表（Event Graph）中，可以为Widget添加交互行为。例如，为按钮添加点击事件：

```c++
UButton* MyButton = WidgetTree->ConstructWidget<UButton>(UButton::StaticClass(), TEXT("MyButton"));
MyButton->OnClicked.AddDynamic(this, &UMyWidgetClass::OnMyButtonClick);
```

编写对应的事件处理函数`OnMyButtonClick`。

### 在关卡中使用UMG Widget

#### 1.**创建Widget实例**

在C++或蓝图中创建Widget实例，并将其添加到视口（Viewport）：

UMyWidgetClass* MyWidget = CreateWidget<UMyWidgetClass>(GetWorld(), UMyWidgetClass::StaticClass());
MyWidget->AddToViewport();

#### 2.**交互和数据绑定**

可以通过绑定将数据传递到Widget中。例如，绑定玩家的健康值到进度条：

ProgressBar->SetPercent(PlayerHealth / MaxHealth);

### 常用Widget类型

**Button**：按钮，用于触发点击事件。

**Text Block**：文本块，用于显示文本。

**Image**：图像，用于显示图片。

**Progress Bar**：进度条，用于显示进度或数值。

**Slider**：滑块，用于调整数值。

### 示例：创建一个简单的UI

#### 1.**设计界面**

- 创建一个新的Widget Blueprint，命名为`MyHUD`。
- 在画布上添加一个文本块，设置初始文本为“Hello, Unreal!”。
- 添加一个按钮，设置按钮的文本为“Click Me”。

#### 2.**添加行为**

- 在事件图表中，为按钮添加点击事件。在点击事件中，改变文本块的内容：

```c++
void UMyHUD::NativeConstruct()
{
    Super::NativeConstruct();
    

if (MyButton)
{
    MyButton->OnClicked.AddDynamic(this, &UMyHUD::OnButtonClick);
}

}

void UMyHUD::OnButtonClick()
{
    if (MyTextBlock)
    {
        MyTextBlock->SetText(FText::FromString("Button Clicked!"));
    }
}
```

#### 3.**在关卡中显示UI**

在玩家控制器或角色类中，创建并添加Widget实例到视口：

```c++
void AMyPlayerController::BeginPlay()
{
    Super::BeginPlay();
    

UMyHUD* MyHUDWidget = CreateWidget<UMyHUD>(this, MyHUDClass);
if (MyHUDWidget)
{
    MyHUDWidget->AddToViewport();
}

}
```

UMG（Unreal Motion Graphics）是虚幻引擎（Unreal Engine）中用于创建用户界面（UI）的工具。它提供了强大的功能，可以帮助开发者快速创建和管理游戏中的UI元素，如按钮、文本框、图像等。以下是UMG的一些关键概念和使用方法的详解：

### 关键概念

1. **Widget**：UMG中的每个UI元素都称为一个Widget。Widget可以是按钮、文本、图像、进度条等。
2. **Widget Blueprint**：这是用来设计和实现UI的主要工具。通过Widget Blueprint，开发者可以可视化地拖放Widget，并为它们添加行为。
3. **Widget Hierarchy**：Widget可以嵌套在一起，形成一个层次结构。例如，一个按钮可以包含一个文本Widget。
4. **Bindings**：UMG允许绑定数据到Widget，以便动态更新UI。例如，可以绑定玩家的分数到一个文本Widget，实时显示分数变化。

### 创建UMG Widget

1. **创建Widget Blueprint**：

   - 右键单击内容浏览器，选择`User Interface`，然后选择`Widget Blueprint`。
   - 为新的Widget Blueprint命名并打开它。

2. **设计UI**：

   - 在Widget Blueprint编辑器中，可以看到画布（Canvas Panel）和工具面板。
   - 从工具面板中拖放不同的Widget到画布上，如按钮（Button）、文本（Text Block）、图像（Image）等。
   - 可以调整每个Widget的属性，如位置、大小、颜色等。

3. **添加行为**：

   - 在事件图表（Event Graph）中，可以为Widget添加交互行为。例如，为按钮添加点击事件：

     ```c++
     cpp复制代码UButton* MyButton = WidgetTree->ConstructWidget<UButton>(UButton::StaticClass(), TEXT("MyButton"));
     MyButton->OnClicked.AddDynamic(this, &UMyWidgetClass::OnMyButtonClick);
     ```

   - 编写对应的事件处理函数`OnMyButtonClick`。

### 在关卡中使用UMG Widget

1. **创建Widget实例**：

   - 在C++或蓝图中创建Widget实例，并将其添加到视口（Viewport）：

     ```c++
     cpp复制代码UMyWidgetClass* MyWidget = CreateWidget<UMyWidgetClass>(GetWorld(), UMyWidgetClass::StaticClass());
     MyWidget->AddToViewport();
     ```

2. **交互和数据绑定**：

   - 可以通过绑定将数据传递到Widget中。例如，绑定玩家的健康值到进度条：

     ```c++
     cpp
     复制代码
     ProgressBar->SetPercent(PlayerHealth / MaxHealth);
     ```

### 常用Widget类型

1. **Button**：按钮，用于触发点击事件。
2. **Text Block**：文本块，用于显示文本。
3. **Image**：图像，用于显示图片。
4. **Progress Bar**：进度条，用于显示进度或数值。
5. **Slider**：滑块，用于调整数值。

### 示例：创建一个简单的UI

1. **设计界面**：

   - 创建一个新的Widget Blueprint，命名为`MyHUD`。
   - 在画布上添加一个文本块，设置初始文本为“Hello, Unreal!”。
   - 添加一个按钮，设置按钮的文本为“Click Me”。

2. **添加行为**：

   - 在事件图表中，为按钮添加点击事件。在点击事件中，改变文本块的内容：

     ```c++
     cpp复制代码void UMyHUD::NativeConstruct()
     {
         Super::NativeConstruct();
         
         if (MyButton)
         {
             MyButton->OnClicked.AddDynamic(this, &UMyHUD::OnButtonClick);
         }
     }
     
     void UMyHUD::OnButtonClick()
     {
         if (MyTextBlock)
         {
             MyTextBlock->SetText(FText::FromString("Button Clicked!"));
         }
     }
     ```

3. **在关卡中显示UI**：

   - 在玩家控制器或角色类中，创建并添加Widget实例到视口：

     ```c++
     cpp复制代码void AMyPlayerController::BeginPlay()
     {
         Super::BeginPlay();
         
         UMyHUD* MyHUDWidget = CreateWidget<UMyHUD>(this, MyHUDClass);
         if (MyHUDWidget)
         {
             MyHUDWidget->AddToViewport();
         }
     }
     ```

通过UMG，开发者可以轻松地创建复杂的用户界面，并通过绑定和事件处理实现动态交互。UMG的强大之处在于其直观的编辑器和灵活的代码集成，使得UI开发变得更加高效和便捷。

## Cast

### 动态类型转换 (`dynamic_cast`)

```c++
class Base {
    virtual void foo() {}
};

class Derived : public Base {
    void foo() override {}
};

void example(Base* basePtr) {
    Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);
    if (derivedPtr) {
        // 如果转换成功，则可以安全地使用derivedPtr
        derivedPtr->foo();
    }
}
```

### 静态类型转换 (`static_cast`)

```c++
void example(float f) {
    int i = static_cast<int>(f);
    // i 现在是浮点数f的整数部分
}
```

### 强制类型转换 (`reinterpret_cast`)

```c++
void example() {
    int* p = new int(65);
    char* c = reinterpret_cast<char*>(p);
    // 现在c指向的内存位置包含一个字符'A'，因为65的ASCII值是'A'
}
```

### 常量类型转换 (`const_cast`)

```c++
void example(const int* ptr) {
    int* modifiablePtr = const_cast<int*>(ptr);
    *modifiablePtr = 42; // 现在可以修改ptr指向的值
}
```

### 游戏开发中的具体应用

#### 处理游戏对象

```c++
class AActor {};
class ACharacter : public AActor {};
class AWeapon : public AActor {};

void HandleActor(AActor* actor) {
    if (ACharacter* character = dynamic_cast<ACharacter*>(actor)) {
        // 处理角色对象的逻辑
    } else if (AWeapon* weapon = dynamic_cast<AWeapon*>(actor)) {
        // 处理武器对象的逻辑
    }
}
```

#### Unreal Engine 4 中的应用

在UE4中，经常需要将基类指针转换为具体类型，以调用特定的功能。例如：

```c++
AActor* actor = GetWorld()->SpawnActor<AActor>(ActorClass, Location, Rotation);
if (ACharacter* character = Cast<ACharacter>(actor)) {
    // 现在可以调用ACharacter特有的方法
    character->Jump();
}
```

总结来说，`cast`操作在处理多态对象、执行类型安全的转换以及在接口和实现类之间进行转换时非常有用。在游戏开发中，`cast`操作帮助开发者编写更加灵活和通用的代码。

## TSubclassOf

在 Unreal Engine 中，`TSubclassOf` 是一个模板类，用于保存指向某个类的类类型信息，并限制其为指定基类的子类。`TSubclassOf` 常用于表示某种类型的类（而不是类的实例），比如在蓝图或 C++ 中指定可以生成的对象类型。

### 使用场景

**类类型存储**：`TSubclassOf`通常用于存储一个类类型，以便稍后可以使用它来生成对象实例。

**蓝图中设置类**：在蓝图中，可以使用`TSubclassOf`变量来指定要生成的对象的类。

**多态支持**：确保只能存储特定基类的子类类型，提供了类型安全性和灵活性。

### 示例代码

```c++
// Weapon.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Weapon.generated.h"

UCLASS()
class MYGAME_API AWeapon : public AActor
{
    GENERATED_BODY()

public:
    // 构造函数
    AWeapon();
};

// Rifle.h
#pragma once

#include "CoreMinimal.h"
#include "Weapon.h"
#include "Rifle.generated.h"

UCLASS()
class MYGAME_API ARifle : public AWeapon
{
    GENERATED_BODY()

public:
    // 构造函数
    ARifle();
};

// Pistol.h
#pragma once

#include "CoreMinimal.h"
#include "Weapon.h"
#include "Pistol.generated.h"

UCLASS()
class MYGAME_API APistol : public AWeapon
{
    GENERATED_BODY()

public:
    // 构造函数
    APistol();
};

// Character.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "Weapon.h"
#include "Character.generated.h"

UCLASS()
class MYGAME_API ACharacter : public ACharacter
{
    GENERATED_BODY()

public:
    // 构造函数
    ACharacter();

protected:
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Weapons")
    TSubclassOf<AWeapon> DefaultWeaponClass;.

virtual void BeginPlay() override;

};

// Character.cpp
#include "Character.h"
#include "Engine/World.h"

ACharacter::ACharacter()
{
    // 设置默认武器类为ARifle
    DefaultWeaponClass = ARifle::StaticClass();
}

void ACharacter::BeginPlay()
{
    Super::BeginPlay();

// 生成默认武器实例
if (DefaultWeaponClass)
{
    AWeapon* SpawnedWeapon = GetWorld()->SpawnActor<AWeapon>(DefaultWeaponClass, FVector::ZeroVector, FRotator::ZeroRotator);
    if (SpawnedWeapon)
    {
        UE_LOG(LogTemp, Warning, TEXT("Weapon spawned: %s"), *SpawnedWeapon->GetName());
    }
}

}
```

## FORCEINLINE

`FORCEINLINE` 是一个宏定义，用于强制内联函数。在 Unreal Engine 以及其他 C++ 项目中，内联函数是一种将函数代码嵌入到每个调用点的优化技术，而不是在运行时通过函数调用进行跳转。

### 内联函数的好处

减少函数调用开销 内联函数通过消除函数调用的开销，减少了程序的运行时间。

代码优化 编译器可以对内联函数进行更多的优化，因为函数代码直接嵌入在调用点。

### `FORCEINLINE` 的用法

在 Unreal Engine 中，`FORCEINLINE` 宏用于提示编译器将一个函数内联。虽然编译器最终决定是否实际进行内联，但使用 `FORCEINLINE` 可以增加编译器进行内联的可能性。

### 示例

```c++
class MyClass
{
public:
    FORCEINLINE int Add(int a, int b) const
    {
        return a + b;
    }
};
```

### 实现原理

```c++
#if defined(__GNUC__) || defined(__clang__)
    #define FORCEINLINE inline __attribute__ ((always_inline))
#elif defined(_MSC_VER)
    #define FORCEINLINE __forceinline
#else
    #define FORCEINLINE inline
#endif
```

根据不同的编译器，`FORCEINLINE` 被定义为适当的强制内联指令。例如：

在 GNU 编译器（GCC）或 Clang 编译器中，`FORCEINLINE` 被定义为 `inline __attribute__ ((always_inline))`。

在 Microsoft Visual Studio 编译器（MSVC）中，`FORCEINLINE` 被定义为 `__forceinline`。



`VisibleAnywhere`：只显示，不可编辑。

`EditDefaultsOnly`：仅在类默认值中可编辑。

`EditAnywhere`：在类默认值和实例中均可编辑。
