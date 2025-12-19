# **STM32F103C8T6 新手入门与项目实战**

---

## **第一部分：启程准备——"从零到一"**

### **第1章：认识你的新朋友（硬件介绍）**

- #### 1.1 什么是单片机？STM32是什么？
  
  单片机（Microcontroller Unit, MCU）就像一个小小的电脑大脑，能控制灯亮灭、读取传感器数据、与电脑通信等。它集成CPU、内存、I/O引脚于一体，广泛用于智能设备如无人机、汽车电子、智能家居。
  
  STM32是ST Microelectronics（意法半导体）推出的32位ARM Cortex-M系列单片机家族。我们用的是STM32F103C8T6型号：F代表通用型，103是系列，C8表示48引脚、64KB Flash、20KB SRAM，T6是封装类型。它运行在72MHz，适合入门项目。

- #### 1.2 你的开发板长什么样？
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-14-28-09-image.png)
  
  Keysking STM32学习套件板子上集成了多种外设：RGB LED（彩色灯）、按键、旋转编码器、温湿度传感器（AHT20）、OLED显示屏、电位器、无源蜂鸣器、继电器、USB Type-C等。参考 “学习板引脚功能说明.pdf” 中的编号图。
  
  同时，学习套件还附带了超声波模块、电机驱动（DRV8833）、NTC温度传感器、舵机、蓝牙模块、

- #### 1.3 都需要准备哪些工具？（电脑、USB线、几根杜邦线）
  
  电脑：Windows 10/11系统，推荐8GB+内存。
  
  USB线：Type-C线，用于供电和下载程序（套件附带了一根）。
  
  ST-LINK和调试线（套件附带）。
  
  杜邦线：几根杜邦线，用于连接外部模块（套件附带）。
  
  可选：多功能表（万用表），用于测电压；示波器，用于分析I2C通信时序。

- #### 1.4 最重要的第一步：为开发板供电（成功点亮电源灯）
  
  用Type-C USB线或ST-Link连接板子的USB口到电脑。板子上电源指示灯应亮起。如果不亮，检查线缆或USB口。参考原理图Sheet 2：电源部分使用低噪声3.3V稳压和防倒灌电路，确保安全供电。成功供电后，板子已就绪！

### **第2章：搭建你的"工作台"（环境介绍）**

以下软件和应用程序可以找 **电实241樊彧** 获取

- #### 2.1 安装"代码写作软件"（Keil MDK的安装）
  
  Keil MDK（Microcontroller Development Kit）是写C代码、编译、调试的IDE。下载官网最新版。安装时选择ARM组件，输入许可证。安装后，打开Keil，检查是否能新建项目。
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-05-12-image.png)
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-06-30-image.png)
  
  不过由于我们只是学习stm32，所以我们使用学习版（也就是盗版）也问题不大。安装软件可以参考江协科技的视频[STM32入门教程-2023版 细致讲解 中文字幕_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1th411z7sn)

- #### 2.2 安装"芯片图形配置器"（STM32CubeMX）
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-07-30-image.png)
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-08-47-image.png)
  
  STM32CubeMX是ST的免费工具，用于图形化配置引脚、时钟、外设。下载官网版本，安装Java运行环境（如果提示）。打开后，选择STM32F103C8T6芯片。
  
  软件可以在官网下载：[STM32CubeMX | Software - 意法半导体STMicroelectronics](https://www.st.com.cn/zh/development-tools/stm32cubemx.html)

- #### 2.3 安装"快递员驱动"（ST-LINK驱动程序）
  
  部分电脑已经自带驱动程序了，不需要安装。如果之后编写的程序无法下载，再考虑安装驱动程序

- #### 2.4 连接你的开发板
  
  用USB线连接板子到电脑。打开STM32CubeMX，新建项目，选择STM32F103C8T6。直接生成代码，选择Keil MDK格式。打开Keil，加载项目，连接ST-LINK，尝试下载空程序。如果成功，恭喜环境搭建完成！

另外，STM32CubeIDE是一个**一站式的免费、跨平台的集成开发环境**，不需要安装多个软件，也不需要购买，有余力的话可以下载安装试一下，但我们仍然推荐STM32CubeMX与keil的组合

---

## **第二部分：初试身手——"看到光，感受电"（GPIO的输入与输出）**

### **第3章：点亮一个LED**

- #### 3.1 认识"代码开关"：用CubeMX创建一个工程
  
  打开CubeMX，点击上方的File，点击New Project
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-11-56-image.png)
  
  选择/在左上角搜索STM32F103C8T6
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-12-44-image.png)
  
  可以点击`★`来方便下次新建项目时选择
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-13-41-image.png)
  
  然后我们点击右上角的Start Project。
  
  稍等一会，就能来到引脚设置界面：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-15-19-image.png)
  
  我们先点击左侧的System Core，然后点下面弹出的SYS，在右侧弹出的界面的Debug栏选为Serial Wire
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-17-39-image.png)
  
  然后我们点击左侧的RCC，在右侧弹出的界面找到High Speed Colck (HSE)，将其选为晶振
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-19-37-image.png)
  
  然后我们在上方找到Clock Configuration，设置系统时钟（HCLK）为72MHz，然后按一下回车，会弹出这个窗口，问我们要不要切换到其他时钟源
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-22-21-image.png)
  
  我们点OK，稍后他自己就配置完成了，如图
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-23-28-image.png)
  
  然后我们点上方的Project Manager
  
  我们先看Project Settings那一栏：
  
  - 在 Project Name 写项目名称，我们先写“LED_Project”
  
  - 在 Project Location 选择项目存放的位置
  
  - 在 Toolchain / IDE 选择 MDK-ARM，MDK-ARM在这里指的就是keil
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-33-43-image.png)
  
  然后我们点左侧的Code Generater
  
  在Generated Files栏，我们勾选上“为每个外设生成一对'.c/.h'文件”
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-35-50-image.png)
  
  然后我们点右上角的GENERATE CODE，稍等一会会弹出一个弹窗
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-38-15-image.png)
  
  意思是代码已经生成完成了，我们点Open Folder
  
  可以看到大致的目录结构：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-19-44-24-image.png)
  
  ```md
  LED_Project                  项目根目录
  ├─ Core                      用户代码核心目录
  │  ├─ Inc                    存放用户自定义的头文件（.h）
  │  └─ Src                    存放用户自定义的源文件（.c）
  ├─ Drivers                   驱动库目录
  │  ├─ CMSIS                  ARM Cortex 内核相关的底层驱动（如启动文件、内核寄存器定义）
  │  └─ STM32F1xx_HAL_Driver   STM32F1 系列的 HAL 库驱动
  │     ├─ Inc                 HAL 库的头文件
  │     └─ Src                 HAL 库的源文件
  └─ MDK-ARM                   Keil MDK 开发环境的工程文件目录（包含.uvprojx 等工程配置文件）
  ```

- #### 3.2 "指指点点"配置一个GPIO引脚（找到原理图，设置控制LED的引脚为输出模式）
  
  **GPIO是什么**：
  
  - GPIO是通用输入输出（General-Purpose Input/Output）的英文缩写。它是一个芯片上的物理引脚，同时也是你程序中的一个控制对象，我们可以用代码独立的配置每一个GPIO引脚的输入或输出模式。
  
  **GPIO有哪些输出模式？**
  
  - **推挽输出**：可主动输出强高/低电平，驱动能力较强。
  
  - **开漏输出**：只能主动拉低电平，高电平状态需靠外部上拉电阻实现，常用于总线通信。
  
  **GPIO有哪些输入模式？**
  
  - **内部上拉/下拉输入**：内部连接电阻至电源或地，确保引脚在悬空时有一个确定的默认电平。
  
  - **浮空输入**：引脚处于高阻抗状态，芯片内部不连接上拉或下拉电阻，完全由外部电路决定其电压。必须外部有明确的上拉或下拉电阻来确定默认状态，否则电平会不确定。
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-15-45-37-image.png)
  
  参考引脚说明和原理图，RGB LED蓝色用`PA6`。在CubeMX里，直接点击芯片图标的`PA6`引脚，选择为GPIO_Output。
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-20-35-44-image.png)
  
  别忘记先保存（按一下ctrl+s），然后点击右上角GENERATE CODE重新生成一下代码

- #### 3.3 生成代码，打开Keil工程
  
  我们打开项目文件夹，进入MDK-ARM文件夹，然后双击`LED_Project.uvprojx`来打开生成的keil项目。

- #### 3.4 写下第一行"命令"：`HAL_GPIO_WritePin()` 让LED亮起
  
  现在，我们终于要写代码了！打开Keil工程，双击左侧Project窗口中的 **main.c** 文件（通常在Application/User/Core文件夹下）
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-20-08-42-image.png)
  
  找到 main() 函数 `int main(void)`
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-20-10-50-image.png)
  
  main() 函数的**大致**结构是这样的（CubeMX自动生成，不一定一摸一样）：
  
  ```c
  int main(void)
  {
  
    /* USER CODE BEGIN 1 */
  
    /* USER CODE END 1 */
  
    /* MCU Configuration--------------------------------------------------------*/
  
    /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
    HAL_Init();
  
    /* USER CODE BEGIN Init */
  
    /* USER CODE END Init */
  
    /* Configure the system clock */
    SystemClock_Config();
  
    /* USER CODE BEGIN SysInit */
  
    /* USER CODE END SysInit */
  
    /* Initialize all configured peripherals */
    MX_GPIO_Init();
    /* USER CODE BEGIN 2 */
  
    /* USER CODE END 2 */
  
    /* Infinite loop */
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  }
  ```
  
  **关于 while (1) 的解释**：
  
  - 单片机上电后，程序从 main() 函数开始执行。
  - 初始化部分（HAL_Init、时钟、GPIO等）只执行一次。
  - 执行完初始化后，程序进入 `while (1)` 这个**无限循环**。
  - 为什么是无限循环？因为单片机不像电脑程序那样运行完就结束，它需要**一直运行**，持续响应外部事件（如按键、传感器）、控制外设（如灯、电机）。如果没有这个循环，程序执行完初始化就“结束了”，什么都不干了。
  - CubeMX特意在代码里留了很多注释对，其中循环里的注释对为`/* USER CODE BEGIN WHILE */`和`/* USER CODE END WHILE */`，方便我们添加自己的代码——**我们所有的代码都应该写在这些注释对里**。
  - 我们将其称为`主循环`
  
  现在，我们要在`主循环`循环里面添加代码，让蓝色LED亮起。
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-15-45-37-image.png)
  
  根据学习板引脚功能说明和学习板电路原理图：
  
  - 蓝色LED连接到 `PA6`
  - LED是**高电平点亮**（即引脚输出3.3V时LED亮，输出0V时灭）。这是因为LED一端接地（GND），另一端通过电阻接MCU引脚。
  
  添加以下代码，放在`USER CODE BEGIN WHILE`下面：
  
  ```c
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);
  ```
  
  完整代码片段示例：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);  // 点亮蓝色LED
      // 如果你想让它一直亮，就只写这一行就够了
      // （因为在无限循环中反复执行同一命令，效果就是持续亮）
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  我们逐部分详细解释这行代码：
  
  ```c
  HAL_GPIO_WritePin(  参数1  ,  参数2  ,  参数3  );
  ```
  
  - HAL_GPIO_WritePin：这是HAL库提供的一个函数，意思是“写GPIO引脚状态”。它专门用来控制某个引脚输出高电平或低电平。
  
  - 参数1：`GPIOA`
    表示我们要操作的GPIO端口是A端口（STM32有`GPIOA`、`GPIOB`、`GPIOC`等）。控制蓝色LED的PA6属于`GPIOA`，所以这里写`GPIOA`。CubeMX在 main.h 或 stm32f1xx_hal_gpio.h 中已经定义好了这些宏。
  
  - 参数2：`GPIO_PIN_6`
    表示具体哪个引脚。STM32的每个端口有16个引脚，编号从`PIN_0`到`PIN_15`。这里是第6号引脚（`PA6`），即`GPIO_PIN_6`。
  
  - 参数3：`GPIO_PIN_RESET` 表示要把引脚电平设置为**低电平（0）**。
    HAL库用两个宏定义高低电平：
    
    - `GPIO_PIN_RESET` → 0V（低电平）
    
    - `GPIO_PIN_SET` → 3.3V（高电平）
      
      因为我们的蓝色LED是**高电平点亮**，所以用 `GPIO_PIN_SET`。
  
  > **`HAL_GPIO_WritePin()`函数定义**
  > 
  > ```c
  > void HAL_GPIO_WritePin(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin, GPIO_PinState PinState)
  > //                                 参数1↑             参数2↑                  参数3↑
  > ```
  > 
  > 三个参数与上面说的相同
  
  如果你的LED是低电平点亮（多数板子都是低电平点亮），就把参数三改成 `GPIO_PIN_RESET`。
  
  **小贴士：**
  
  - 写完代码后，记得按 Ctrl+S 保存。
  
  - 如果想让LED灭掉，可以写：
    
    ```c
    HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);
    ```
  
  - 还有更方便的函数：`HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_6)`，它可以翻转PA6当前的GPIO状态（亮→灭，灭→亮），后面闪烁实验会用到。

- #### 3.5 编译、下载、见证奇迹！（看到LED点亮）
  
  按F7或点击上方的Build按钮来编译，无报错后，检查一下ST-LINK连接，按下载按钮（图标是一个Load和两个向下的箭头）。下载成功，按一下学习版上的复位按钮，蓝色LED亮起。
  
  以后不特殊说明，使用keil下载程序后都需要手动按一下复位按钮

### **第4章：让LED闪烁**

- #### 4.1 什么是"延时"？`HAL_Delay()` 函数
  
  在上一章，我们让LED一直亮着。但要让它闪烁，就需要“亮一会儿，灭一会儿”。由此，我们可以简单构思一段代码：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);  // 点亮蓝色LED
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);  // 熄灭蓝色LED
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  我们编译下载试一下
  
  诶，为什么蓝色LED还是一直点亮，没有出现闪烁？
  
  其实是因为单片机运行速度极快（我们使用的mcu可以达到最高72MHz），如果不故意暂停，亮灭切换会快到人眼看不出。
  
  由此，HAL库提供了一个超级简单的延时函数：`HAL_Delay(参数)`
  
  - 参数是毫秒数（ms），比如 `HAL_Delay(500)` 就是暂停500毫秒（0.5秒）。
  
  - 它基于系统滴答定时器（SysTick），比较准确，适合入门使用。
  
  - 注意：这个函数只能在初始化完成后使用（因为它依赖HAL_Init()）。

- #### 4.2 你的第一个程序：LED闪烁（经典的"Hello World"）
  
  现在，我们来写单片机界的“Hello World”——LED闪烁程序！这几乎是所有嵌入式开发的第一个里程碑。
  
  我们继续用蓝色LED（`PA6`）作为例子。
  
  在 main.c 的`主循环`中，修改代码如下：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);  // 点亮蓝色LED
      HAL_Delay(500);    // 延迟500毫秒
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);  // 熄灭蓝色LED
      HAL_Delay(500);
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  运行效果：
  
  - LED亮0.5秒 → 灭0.5秒 → 亮0.5秒 → 灭0.5秒 → …… 无限循环，形成规律闪烁。
  
  - 闪烁周期是1秒（500ms + 500ms），频率是1Hz（每秒闪烁一次）。
  
  另一种写法：
  
  我们可以使用上面介绍过的`HAL_GPIO_TogglePin()`来翻转PA6的电平，而不是设置电平
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_6);  // 翻转一次
      HAL_Delay(500);    // 延迟500毫秒
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  别忘了编译下载

- #### 4.3 改变闪烁的频率（快闪 vs 慢闪）
  
  闪烁频率就是“多快亮灭一次”，完全由 `HAL_Delay()` 的参数决定。
  
  - 实验1：慢闪（2秒周期）
    
    我们只需要把延时改大即可：
    
    ```c
      /* USER CODE BEGIN WHILE */
      while (1)
      {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_6);
        HAL_Delay(1000);  // 改成1000毫秒 = 1秒
    
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    ```
    
    效果：亮1秒 → 灭1秒 → …… 周期2秒，很慢。
  
  - 实验2：快闪（0.2秒周期）
    
    你先来自己试一试吧
    
    代码：
    
    ```c
      /* USER CODE BEGIN WHILE */
      while (1)
      {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_6);
        HAL_Delay(100);  // 改成100毫秒 = 0.1秒
    
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    ```
    
    此时周期为0.1+0.1=0.2秒
  
  - 实验3：不对称闪烁（亮100毫秒，灭900毫秒）
    
    代码：
    
    ```c
      /* USER CODE BEGIN WHILE */
      while (1)
      {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);   // 亮
        HAL_Delay(100);                                       // 亮100ms
    
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET); // 灭
        HAL_Delay(900);                                       // 灭900ms
    
        /* USER CODE END WHILE */
    
        /* USER CODE BEGIN 3 */
      }
      /* USER CODE END 3 */
    ```

### **第5章：按键输入（轮询方式）**

- #### 5.1 认识"输入"模式：用CubeMX配置按键引脚
  
  我们先打开项目目录，双击`LED_Project.ioc`打开CubeMX
  
  我们观察一下原理图：两个按键分别连接`PB12`和`PB13`
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-20-26-46-image.png)
  
  我们先忽略按键并联的电容，可以看到`KEY1`左侧连接`PB12`和一个10KΩ的电阻，电阻另一端连接3.3V。这样
  
  - 当**按下**按钮，`PB12`直接连接GND，此时为**低电平**（电压为0）
  
  - 当**松开**按钮，`PB12`通过电阻连接到3.3V，此时为**高电平**（电压为3.3V）
  
  再看`KEY2`，它没有连接电阻，所以
  
  - 当**按下**按钮，`PB13`直接连接GND，此时为**低电平**（电压为0）
  
  - 当**松开**按钮，`PB12`浮空（没有连接任何元件），此时电平**不确定**
  
  所以我们得出结论：`KEY1`配备一个上拉电阻，`KEY2`没有上拉/下拉电阻。当我们想使用`KEY2`时，需要配置**内部上拉**。
  
  > **关于按键并联电容的作用**
  > 
  > 事实上，我们按下按键的那一刻并**不会立刻完全短接**按键两端，而是由于材料与机械的限制，出现短暂的 *连接* - *断开* - *连接* 的**抖动**，松开按键也同样会出现这样的**抖动**。这个抖动时间极短，大概只有几毫秒至十几毫秒，虽然人眼看不出来，但也会被单片机识别到。
  > 
  > 我们高中物理学过：**电容器可以抑制其两端电压的变化**，所以让按钮并联一个电容能**比较**有效地减小该抖动。当然，这**不能完全拦截**抖动，所以我们还需要在程序里实现**消除抖动的算法（消抖）**
  
  我们先使用`KEY1`，所以我们在CubeMX界面点击芯片图标的PB12引脚，选择GPIO_Input
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-15-20-42-41-image.png)
  
  然后按ctrl+s，并重新生成代码。

- #### 5.2 读取按键状态并控制LED（按下亮，松开灭）：`HAL_GPIO_ReadPin()`
  
  对于读取GPIO状态，HAL库提供了专门的函数：`HAL_GPIO_ReadPin()`
  
  函数原型：
  
  ```c
  GPIO_PinState HAL_GPIO_ReadPin(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
  //                                         参数1↑             参数2↑
  ```
  
  - 参数1：GPIOB（因为PB12/PB13属于B端口）
  
  - 参数2：GPIO_PIN_12 或 GPIO_PIN_13
  
  - 返回值：
    
    - `GPIO_PIN_SET` → 高电平（对于`KEY1`：没按下）
    
    - `GPIO_PIN_RESET` → 低电平（对于`KEY2`：按下）
  
  由此，我们可以先写一段简单的代码来测试单片机对GPIO的读取功能：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)  // 如果KEY1按下
      {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);     // 点亮蓝色LED
      }
      else
      {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);   // 熄灭蓝色LED
      }
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  这个代码只是临时测试，后面会优化。我们用 KEY1 (PB12) 和蓝色LED (PA6) 作为例子。
  
  **注意**：
  
  - 读取前在CubeMX中把PB12配置为 **GPIO_Input**。
  - 读取函数非常快，可以在循环中不断调用，这就是“轮询”（polling）方式。
  
  编译下载，按下`KEY1`，蓝色LED亮起；松开`KEY1`，蓝色LED熄灭。
  
  那我们想用`KEY2`控制LED怎么办呢？
  
  我们需要在CubeMX里配置一下5.1节里提到的**内部上拉**：
  
  点击PB13，选择GPIO_Input
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-17-33-33-image.png)
  
  然后点击左侧的System Core，选择展开的GPIO，可以看到右侧弹出的窗口下方有各个启用了GPIO功能的引脚和相关配置：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-17-36-15-image.png)
  
  我们点一下下方的PB13，会弹出一个详细的配置窗口：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-17-37-40-image.png)
  
  可以看到，GPIO Pull-up/Pull-down那一栏当前为No pull-up and no pull-down，意思是当前没有内部上拉和内部下拉，也就是浮空输入模式。
  
  所以我们只需要在GPIO Pull-up/Pull-down那一栏，选择Pull-up即可启用内部上拉模式
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-17-41-37-image.png)
  
  然后保存并重新生成代码，将上面的代码中的`GPIO_PIN_12`改为`GPIO_PIN_13`：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)  // 如果KEY1按下
      {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);     // 点亮蓝色LED
      }
      else
      {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);   // 熄灭蓝色LED
      }
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  然后编译下载，按下`KEY2`，蓝色LED亮起；松开`KEY2`，蓝色LED熄灭。

- #### 5.3 项目：按键切换LED状态（按一次开，再按一次关）
  
  这个项目更有实用价值：像家里的灯光开关，按一下开，再按一下关（即“触发式”或“翻转式”）。
  
  直接用上面的“按下亮、松开灭”会不合适——松开后又自动关了。我们需要**记住上一次的状态**，每次按键只翻转一次。
  
  **核心思路**：
  
  - 用一个变量（flag）记录当前LED状态（0=灭，1=亮）
    
    直接在`主循环`外定义一个变量，用于存储LED状态即可。
    
    ```c
      /* USER CODE BEGIN 2 */
      uint8_t led_state = 0;  // 0=灭，1=亮，初始为0，即熄灭
      /* USER CODE END 2 */
    ```
    
    要将上述代码写在`/* USER CODE BEGIN 2 */`和`/* USER CODE END 2 */`中间
  
  - 检测到按键**按下**（从高到低的变化）时，翻转flag，并更新LED
    
    ```c
    if(按键状态 == GPIO_PIN_RESET)
    {
        led_state = !led_state;
    }
    ```
  
  - 要避免长按时反复翻转 → 加入**软件消抖**和**等待松开**
    
    要注意的是，**软件消抖和等待松开**要写在**按键按下翻转flag**外层，因为前者的目的是防止误触发后者。
    
    我们可以这样实现：
    
    ```c
    if(按键状态 == GPIO_PIN_RESET)         // 检测按键是否被按下（低电平）
    {
        HAL_Delay(20);                    // 软件消抖：等20ms，过滤机械抖动
        if(按键状态 == GPIO_PIN_RESET)     // 确认仍为低电平，而不是抖动
        {
            按下翻转和控制LED的逻辑;
        }
        while(按键状态 == GPIO_PIN_RESET)  // 用循环卡住代码，直到按键松开
        {
            HAL_Delay(5);                 //加个延时，减小CPU占用
        }
    }
    ```
  
  综合来看，我们的代码应该是这样的：
  
  ```c
  int main(void)
  {
  
    /* USER CODE BEGIN 1 */
  
    /* USER CODE END 1 */
  
    /* MCU Configuration--------------------------------------------------------*/
  
    /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
    HAL_Init();
  
    /* USER CODE BEGIN Init */
  
    /* USER CODE END Init */
  
    /* Configure the system clock */
    SystemClock_Config();
  
    /* USER CODE BEGIN SysInit */
  
    /* USER CODE END SysInit */
  
    /* Initialize all configured peripherals */
    MX_GPIO_Init();
    /* USER CODE BEGIN 2 */
    uint8_t led_state = 0;
    /* USER CODE END 2 */
  
    /* Infinite loop */
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      // 检测按键是否被按下（低电平）
      if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
      {
        HAL_Delay(20);  // 软件消抖：延时20ms，过滤机械抖动
  
        // 再次确认仍为低电平（确实按下，而不是噪声）
        if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
        {
          led_state = !led_state;  // 翻转状态：0→1，1→0
  
          // 根据新状态控制LED
          if (led_state == 1)
          {
            HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);     // 亮
          }
          else
          {
            HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);   // 灭
          }
  
          // 等待按键松开，防止长按时连续翻转
          while (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
          {
            HAL_Delay(5);
          }
        }
      }
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  }
  ```
  
  或者我们可以使用`HAL_GPIO_TogglePin()`函数
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
      {
        HAL_Delay(20);
  
        if (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
        {
  
          HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_6);  // 直接翻转即可
  
          while (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
          {
            HAL_Delay(5);
          }
        }
      }
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  你只需要保证CubeMX配置正确，`/* USER CODE BEGIN 2 */`和`/* USER CODE BEGIN WHILE */`两个注释对中的代码与我的一样即可
  
  编译下载，此时我们按一下`KEY1`，蓝色LED亮起，再按一下`KEY1`，蓝色LED熄灭。

### **第6章：外部中断——让CPU"立即响应"**

- #### 6.1 轮询 vs 中断：两种工作方式的对比
  
  在前几章，我们用“轮询”（polling）方式实现了按键控制LED：程序`在主循环`里不停地读取按键状态，一旦发现按下就执行相应操作。这对新手来说简单直观，但其实还有一种更聪明、更高效的方式——中断。我们先从概念入手，慢慢带你理解为什么需要中断，以及它是怎么工作的。
  
  我们先来回顾一下你已经熟悉的**轮询**方式，用一个生活中的比喻来说明：
  
  想象你（CPU）是一个勤快的保安，正在负责看守一扇门（按键）。你想知道有人按门铃（按键按下）时立刻开门（点亮LED）。
  
  **轮询方式**就像：
  
  - 你每隔几秒钟就主动跑去门口看一眼：“有人按门铃吗？没有……有人按吗？没有……”
  - 这样确实能发现有人按铃，但大部分时间你都在“白跑”，浪费精力。
  - 如果同时还要干其他事（比如让另一个LED闪烁、读取传感器），你就得在看门和干活之间来回切换，忙得团团转。
  
  在程序里就是：
  
  - `主循环`里不停调用`HAL_GPIO_ReadPin()`检查按键
  
  - CPU 几乎所有时间都花在“问有没有按下”上，即使99%的时间按键都没动
  
  - 优点：代码简单，新手容易理解和调试
  
  - 缺点：
    
    - 浪费CPU资源（单片机本来计算能力就不强）
    
    - 响应可能有延迟（如果循环里还有其他耗时操作，比如长延时，可能会好几百毫秒才检查到按键）
    
    - 不适合多任务场景
  
  **中断方式**就像给门铃装了一个电线，直接连到你的手机上：
  
  - 你平时可以安心干别的活（执行主程序、闪烁LED、采集数据等）
  - 只要有人一按门铃，手机立刻响（产生中断信号），你**马上放下手头的事**去开门
  - 开完门后再继续刚才没干完的活
  
  在程序里就是：
  
  - CPU 平时正常运行主循环
  - 一旦按键按下，硬件自动向CPU发送一个“紧急通知”（中断请求）
  - CPU **立即暂停**当前程序，跳去执行一个专门的“按键处理函数”
  - 处理完后再回到原来被打断的地方，继续执行
  
  **对比表格**（方便记忆）：
  
  | 项目    | 轮询（Polling）      | 中断（Interrupt）    |
  | ----- | ---------------- | ---------------- |
  | 工作方式  | CPU主动反复询问“有没有事？” | 外设主动通知CPU“有事了！”  |
  | CPU占用 | 高（一直在问）          | 低（平时空闲，有事才忙一会儿）  |
  | 响应速度  | 有延迟（取决于循环检查频率）   | 几乎立即（微秒级）        |
  | 代码复杂度 | 简单               | 稍复杂（需要配置中断和回调函数） |
  | 适合场景  | 简单任务、实时要求不高      | 实时性要求高、多任务、省电场景  |
  | 功耗    | 高                | 低（适合电池供电设备）      |
  
  总结：轮询适合入门学习和简单项目，中断是嵌入式开发的“标配”，几乎所有实际产品都大量使用中断（按键、串口接收、定时器、传感器触发等）。

- #### 6.2 什么是外部中断（EXTI）？
  
  STM32 把来自芯片**外部引脚**的变化（比如按键按下导致电平从高到低）产生的中断，叫做**外部中断**，英文简称 **EXTI**（External Interrupt）。
  
  **简单理解**：
  
  - STM32 有专用的 EXTI 模块，能“监视”某些 GPIO 引脚的电平变化
  - 你可以提前告诉它：“请关注 PB12 这个引脚，一旦发现它从高电平变成低电平（下降沿），就立刻通知CPU”
  - 当事件真的发生时，EXTI 硬件会向 CPU 发出中断请求
  - CPU 收到后，自动跳转执行我们写好的“中断处理函数”（也叫回调函数）
  
  **STM32F103C8T6（我们用的芯片）的EXTI线数量和对应关系**：
  
  STM32F103系列的EXTI一共有**16条中断线**，但并不是每条线都对应一个独立的中断向量，而是被“分组”管理（这个理解不了没关系，先跳过就行）：
  
  | 名称                                                                                         | EXTI组     | 对应引脚号            | 说明                                 | 对应中断处理函数             |
  | ------------------------------------------------------------------------------------------ | --------- | ---------------- | ---------------------------------- | -------------------- |
  | GPIO_EXTI0                                                                                 | EXTI0     | 引脚0（PA0/PB0/PC0） | 独立中断                               | EXTI0_IRQHandler     |
  | GPIO_EXTI1                                                                                 | EXTI1     | 引脚1              | 独立中断                               | EXTI1_IRQHandler     |
  | GPIO_EXTI2                                                                                 | EXTI2     | 引脚2              | 独立中断                               | EXTI2_IRQHandler     |
  | GPIO_EXTI3                                                                                 | EXTI3     | 引脚3              | 独立中断                               | EXTI3_IRQHandler     |
  | GPIO_EXTI4                                                                                 | EXTI4     | 引脚4              | 独立中断                               | EXTI4_IRQHandler     |
  | GPIO_EXTI5<br>GPIO_EXTI6<br>GPIO_EXTI7<br>GPIO_EXTI8<br>GPIO_EXTI9<br>                     | EXTI9_5   | 引脚5～9            | **共用一个中断向量**（引脚5-9触发时都进入同一个中断函数）   | EXTI9_5_IRQHandler   |
  | GPIO_EXTI10<br>GPIO_EXTI11<br>GPIO_EXTI12<br>GPIO_EXTI13<br>GPIO_EXTI14<br>GPIO_EXTI15<br> | EXTI15_10 | 引脚10～15          | **共用一个中断向量**（引脚10-15触发时都进入同一个中断函数） | EXTI15_10_IRQHandler |
  
  **举例说明**（我们最常用的按键）：
  
  - PB12 属于**引脚12** → 它连接到 **EXTI12**（属于 EXTI15_10 组）
  - PB13 属于**引脚13** → 也属于 **EXTI15_10** 组
  - 所以如果同时开启 PB12 和 PB13 的外部中断，**它们会共用同一个中断处理函数**（在函数里要通过 GPIO_Pin 参数来判断到底是哪个引脚触发的）。
  
  **关键特点**（新手最容易混淆的点）：
  
  - 同一组内的多个引脚共用一个中断向量（比如 EXTI15_10 对应所有10～15号引脚）
  
  - 同一条 EXTI 线只能被一个端口的同号引脚使用（比如只能是 PA12 或 PB12 或 PC12 中的一个，不能同时用多个）
  
  - 中断的触发方式有：
    
    - **下降沿触发**：高电平 → 低电平（适合按键按下，具体得看原理图）
    
    - **上升沿触发**：低电平 → 高电平（适合按键松开，具体得看原理图）
    
    - **双边沿**：高低变化都触发
  
  - 中断处理函数必须短而快！因为中断是“打断”主程序的，处理太久会影响主程序正常运行
  
  我们在学习中断之前，先来做一个小实验，要求是：
  
  - 蓝色LED（PA6）每隔200ms自动翻转一次（即周期400ms闪烁，不依赖按键）
  
  - 当按下 KEY1（PB12）时，绿色LED（PA7）立刻翻转一次
    即 按一下 → 亮 → 按一下 → 灭
    
    **记得在CubeMX里配置一下PA7的输出！**
  
  你先自己试一下
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);
      HAL_Delay(200);
      HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);
      HAL_Delay(200);
  
      if(HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
      {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_7);
        while (HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET);
        //也可以这样写循环来卡住程序
      }
  
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  下载运行后，你会发现的问题：
  
  - 只有当我们疯狂按`KEY1`或者长按`KEY1`才能点亮绿色LED
  
  - 如果长按点亮了绿色LED后不放手（继续长按），蓝色LED会暂时停止闪烁，直到松手
  
  这就是轮询会导致的问题，之后我会介绍如何用中断解决该问题。

- #### 6.3 用CubeMX配置按键的外部中断
  
  我们打开CubeMX，设置`PB12`为GPIO_EXTI12：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-20-49-35-image.png)
  
  然后同样在左侧的System Core - GPIO里找到并点击PB12：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-20-50-57-image.png)
  
  我们展开（点一下）GPIO mode的下拉框：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-20-52-26-image.png)
  
  其中，前三个与中断有关
  
  | 选项                                                                 | 解释    |
  | ------------------------------------------------------------------ | ----- |
  | External Interrupt Mode with Rising edge trigger detection         | 上升沿触发 |
  | External Interrupt Mode with Falling edge trigger detection        | 下降沿触发 |
  | External Interrupt Mode with Rising/Falling edge trigger detection | 都触发   |
  
  我们在6.2节已经介绍过上升沿和下降沿了，所以我们在这选择下降沿，即按键按下触发中断
  
  然后我们在左侧的System Core里找到并点一下NVIC，在右侧弹出窗口里找到
  EXTI line[15:10] interrupts，在Enabled栏勾选上：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-21-09-05-image.png)
  
  保存并重新生成代码，打开keil，在左侧双击stm32f1xx_it.c
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-21-23-34-image.png)
  
  在该文件的最底部，CubeMX自动帮我们生成了一个函数
  `void EXTI15_10_IRQHandler(void)`：
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-21-24-53-image.png)
  
  这就是我们刚刚配置的中断回调函数。

- #### 6.4 编写中断回调函数并完善程序：`EXTI15_10_IRQHandler(void)`
  
  由于我们只需要实现按下按钮反转`PA7`的电平，所以只需要在
  `/* USER CODE BEGIN EXTI15_10_IRQn 0 */`里写一行
  
  ```c
  HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_7);
  ```
  
  即可。
  
  然后回到main.c，修改`主循环`，使其仅包含蓝色LED闪烁的代码
  
  编译下载试一下，我们发现中断工作正常。吗？
  
  我们多试几次，似乎有时候绿色LED没有翻转亮灭状态，这是为什么呢？
  
  其实是因为我们没有对其进行消抖导致的。
  
  那我们在回调函数`EXTI15_10_IRQHandler(void)`的
  `/* USER CODE BEGIN EXTI15_10_IRQn 0 */`里简单写一个消抖：
  
  ```c
  HAL_Delay(10);
  if(HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_12) == GPIO_PIN_RESET)
  {
      HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_7);
  }
  ```
  
  编译下载，试一下。
  
  按下按键，诶，为什么程序暂停了呢？不仅绿色LED没有亮起，连蓝色LED也不闪了。
  
  这是因为`HAL_Delay()`函数依赖叫做System tick timer (系统滴答) 的中断，能为其提供1毫秒的时间基准，但它的优先级低于我们触发的外部中断，所以其不能在我们的中断处理函数内执行，程序也就卡死在这了。
  
  想要解决，我们只需要回到CubeMX，进入System Core - NVIC，将我们的EXTI line[15:10] interrupts的优先级设为最高（15），同时将Time base: System tick timer的优先级设为一个比咱们的外部中断更低的数字即可
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-17-21-51-34-image.png)
  
  保存并生成代码，回到keil，编译下载，正常运行！
  
  到目前为止，这个项目`LED_Project`基本完善，已经可以告一段落了，之后我们将会新建一个项目来进行讲解。

- #### 6.5 中断的应用场景
  
  我们先前已经介绍过中断的有点了，它的缺点是：
  
  - 增加了程序的复杂性，调试更难
  
  - 还得配置CubeMX
  
  - 中断函数需要简短快速
  
  - 得考虑优先级
  
  应用场景：
  
  - 某些人机交互：按键、触摸检测等
  
  - 定时与计时任务：PWM、定时器更新等
  
  - 异常与紧急事件：硬件错误、电源故障报警等
  
  - 传感器通过专用的中断引脚通知MCU

## **第三部分：核心技能——"掌握沟通的艺术"（串口，定时器与ADC）**

### **第7章：单片机的"嘴巴耳朵"：串口通信**

- #### 7.1 串口是什么？
  
  串口是嵌入式领域最常用的通信方式。常见的串口有：RS-232，RS-485
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-18-08-10-55-image.png)
  
  甚至更常见的RJ-45（网线接口），USB接口也都是串口。
  
  不过，在单片机上，更常用的是TTL串口：其只需要两根线即可完成设备间的双向通信
  
  一根从设备1发送到设备2，另一根从设备2发送到设备1
  
  用来发送的接口叫TX (Transmit)，用来接收的接口叫RX (Receive)
  
  使用时，注意应该把一个设备的TX连接另一设备的RX（一方发送，另一方接收），而不是TX连TX，RX连RX
  
  有时候还需要连接一根GND来保证低电平的参考电压是准确的

- #### 7.2 CubeMX配置串口（USART1）
  
  开始之前，我们先新建一个项目`Serial`（串行）：
  
  打开CubeMX，File - New Project，点一下弹出窗口左上角的★可以看到我们已经收藏的STM32F103C8T6
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-18-13-52-41-image.png)
  
  选择它，然后点右上角的Start Project
  
  依次完成以下操作，如果不记得了可以看3.1节：
  
  - 点击左侧的System Core - SYS，在右侧弹出的界面的Debug栏选为Serial Wire
    这点很重要，因为涉及到IO复用。如果不这样操作可能导致调试接口关闭，从而出现无法下载程序的情况。如果已经出现无法下载的情况，解决方案[报错 No Device Found | 波特律动](https://docs.keysking.com/docs/stm32/FAQ/CompilationFailed)
  
  - 设置RCC的HSE为晶振
  
  - 在上方进入Clock Configuration，设置HCLK为72MHz，按一下回车
  
  - 点上方的Project Manager
    
    - 项目名称写“Serial”
    
    - Project Location 选择项目存放的位置
    
    - Toolchain / IDE 选 MDK-ARM
    
    点左侧的Code Generater
    
    - 勾选上“为每个外设生成一对'.c/.h'文件”
  
  然后生成代码即可
  
  我们在引脚功能说明里可以看到，学习版上的type-c接口的TX（Transmit，发送）与RX（Receive，接收）分别连接PA2和PA3，它们都属于USART2
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-19-11-06-32-image.png)
  
  这里的USART是Universal Synchronous / Asynchronous Receiver & Transmitter，即通用同步/异步接收器和发送器，我们用的TTL串口用的就是其中的Asynchronous（异步）也就是UART
  
  我们打开CubeMX，点左侧的Connectivity，可以看到有三个USART，我们选择USART2，设置模式为Asynchronous
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-19-11-13-22-image.png)
  
  可以看到右侧PA2与PA3都正确设置了
  
  在下方的参数中，需要注意Baud Rate（波特率）这一行
  
  ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-19-11-14-44-image.png)
  
  是指每秒传输的码元数量，即每秒有多少次高低电平的变化
  
  默认情况下，串口每传递一个字节（8位二进制），需要添加一位起始位和一位停止位，也就是传输一字节需要10位二进制，所以115200的波特率一秒可以传递11520字节数据
  
  常见的波特率还有9600、192000、384000等等，两设备需要使用相同的波特率才能通信，这里我们保持默认（115200）就好
  
  保存并生成代码

- #### 7.3 向电脑发送"悄悄话"：`HAL_UART_Transmit()`
  
  我们先来尝试每秒像电脑发送一次“Hello World”
  
  打开keil，我们先在主函数外定义一个字符数组：
  
  ```c
    /* USER CODE BEGIN 2 */
  	char message[] = "Hello World";
    /* USER CODE END 2 */
  ```
  
  然后使用`HAL_UART_Transmit()`函数用串口发送数据：
  
  ```c
    /* USER CODE BEGIN WHILE */
    while (1)
    {
  	HAL_UART_Transmit(&huart2, (uint8_t*)message, strlen(message), 100);
      HAL_Delay(1000);
      /* USER CODE END WHILE */
  
      /* USER CODE BEGIN 3 */
    }
    /* USER CODE END 3 */
  ```
  
  HAL_UART_Transmit() 是 STM32 HAL 库提供的**阻塞式（同步）串口发送函数**。它会把数据通过指定的串口一个字节一个字节地发送出去，**发送完成前函数不会返回**（会一直等待），适合发送数据量不大、不需要立即干其他事的场景。
  
  函数原型（来自 HAL 库）：
  
  ```c
  HAL_StatusTypeDef HAL_UART_Transmit(UART_HandleTypeDef *huart, //参数1
                                      uint8_t *pData,            //参数2
                                      uint16_t Size,             //参数3
                                      uint32_t Timeout);         //参数4
  ```
  
  > 可以看到，这个函数不是由void定义的，也就是说它有返回值，这是很多人忽略的
  > 
  > 之后我会讲一下这个函数的返回值
  
  我们写的是：
  
  ```c
  HAL_UART_Transmit(&huart2, (uint8_t*)message, strlen(message), 100);
                   参数1↑          参数2↑           参数3↑      参数4↑
  ```
  
  我们逐个参数详细解释：
  
  **第1个参数：&huart2**
  
  - **类型**：`UART_HandleTypeDef *huart`（UART 句柄的指针）
  - **含义**：告诉函数你要用**哪个串口**发送数据。
  - **huart2** 是 CubeMX 自动生成的串口2（USART2）的句柄变量。
    - 在 Keysking 学习套件中，板载 USB 转串口芯片（CH343P）连接的就是 **USART2**（TX: PA2，RX: PA3），所以通过 USB 线连接电脑时，我们用 USART2 就能和电脑的串口助手通信。
  - **&huart2**：取 huart2 变量的地址（指针），因为函数需要指针类型。
  
  **第2个参数：(uint8_t*)message**
  
  - **类型**：`uint8_t *pData`（指向要发送数据的指针）
  
  - **含义**：指向你要发送的**数据缓冲区**（字节数组）的起始地址。
  
  - 因为`message`是一个字符数组，所以`message`本身就是char指针类型
  
  - **(uint8_t*)**：强制类型转换，把 char* 转换成 uint8_t*（无符号8位字节指针）。
    
    - 因为串口发送的是一个个字节（8位），HAL 库统一用 uint8_t 处理，不管原数据是 char 还是其他。
    - 同时，char本身也是8位二进制，所以能直接转为uint8_t类型
  
  - 如果不强制转换，编译可能会警告
  
  **第3个参数：strlen(message)**
  
  - **类型**：`uint16_t Size`
  - **含义**：要发送的**字节数量**（数据长度）。
  - **strlen(message)**：C 语言标准函数，计算字符串长度。
    - 例如 "Hello" 的 strlen 是 5。
    - 如果字符串里有 "\r\n"（回车换行），strlen 也会正确计入（\r 是1字节，\n 是1字节）。
  - 你也可以手动写数字，比如发送固定10个字节，就写 10。
  - **注意**：如果用 strlen，记得包含头文件 `#include <string.h>`不然也会警告
  
  **第4个参数：100**
  
  - **类型**：`uint32_t Timeout`
  - **含义**：**超时时间**，单位是**毫秒（ms）**。
  - 函数会等待数据发送完成，但如果因为硬件故障或其他原因一直发送不完，它不会无限卡死，而是等到指定时间后放弃并返回错误。
  - 常用值：
    - **100~1000**：大多数情况够用。
    - **HAL_MAX_DELAY**（定义为 0xFFFFFFFF）：无限等待，直到发送完成
    - **0**：不等待，立刻检查是否能发送（很少用）。
  
  **函数的返回值：**
  
  - 函数返回 HAL_StatusTypeDef 类型：
    
    - HAL_OK：发送成功
    - HAL_ERROR：出错
    - HAL_BUSY：串口忙
    - HAL_TIMEOUT：超时未完成
    
    我们可以用在错误分析上
  
  现在编译下载。但是接下来我们好像遇到了两个问题：
  
  - 如何让stm32与电脑连接？
    
    通常我们需要一个USB转TTL串口的模块，直接与stm32连接，但是学习版上已经集成了一个USB转TTL芯片，可以在原理图上找到
    
    ![](C:\Users\lpg01\AppData\Roaming\marktext\images\2025-12-19-11-44-14-image.png)
    
    看不懂没关系，根据引脚功能说明，学习版使用一个usb type-c接口作为串口连接，所以我们只需要拿出一根type-c的数据线，将学习版与电脑连接即可
  
  - 如何在电脑上看到stm32通过串口发来的消息？
    
    这时候我们就需要一种叫做串口助手的工具，网上还是有很多的。
    
    > 有一个即开即用，浏览器就能用的串口助手：[波特律动 串口助手](https://serial.keysking.com/#/)，之后的教程都将使用这个串口助手
    
    我们只需要点左下角的选择串口，找到学习版对应的串口选择就好了。如果不知道哪个是，我们可以拔下来再插上数据线，观察哪个选项消失又出现，哪个就是学习版的串口。如果没有变化，可能是计算机缺少串口驱动导致的，我们只需要在串口助手左下角点一下小工具，会出现常见驱动下载，学习版使用的是CH343芯片，下载安装即可
  
  不出意外，我们应该能看到每秒钟出现一次“Hello World”，如果不正确，检查一下波特率对不对，检查一下串口助手是否显示ASCII

- #### 7.4 接收电脑的指令：控制LED
  
  

- #### 7.5 串口的中断模式收发

- #### 7.6 串口DMA（直接内存访问）模式

### **第8章：单片机的"脉搏"：定时器**

- #### 8.1 为什么需要定时器？（解放主循环，实现精准时间）

- #### 8.2 配置一个基本定时器（TIM）

- #### 8.3 定时器中断回调函数：让LED定时自动闪烁

- #### 8.4 项目：用定时器制作精准秒表

- #### 8.5 定时器的高级功能：PWM输出

- #### 8.6 项目：用PWM实现呼吸灯/LED调光

- #### 8.7 编码器接口与舵机控制

- #### 8.8 扩展：直流电机与DRV8833驱动

### **第9章：感受模拟世界：ADC读取电压**

- #### 9.1 数字与模拟的区别（比喻：开关 vs 水龙头）

- #### 9.2 配置ADC，读取开发板上的可调电阻（电位器）

- #### 9.3 项目：制作一个电压表（通过串口显示电压值）

- #### 9.4 扩展：根据电位器位置控制LED闪烁频率

## **第四部分：探索进阶——"连接更多设备"**

### **第10章：驱动OLED显示屏（I2C协议）**

- #### 10.1 I2C协议极简介绍（"老师点名，学生答到"）

- #### 10.2 使用HAL库的I2C配置

- #### 10.3 移植OLED显示库，显示字符与位图

- #### 10.4 扩展：读取温湿度计的数据（AHT20）

## **第五部分：系统与幕后——"理解魔法原理"**

### **第11章：深入STM32内部**

- #### 11.1 程序是怎么跑起来的？——浅谈Cortex-M3内核

- #### 11.2 时钟树：芯片的"心脏"与"脉搏"

- #### 11.3 存储器空间：程序住哪里，变量住哪里？（寄存器，Flash和SRAM）

- #### 11.4 中断系统详解：NVIC与中断优先级

## **第六部分：综合项目——"创造你的世界"**

### **第12章：项目实战一：桌面环境监测站**

- #### 整合：OLED显示 + 温湿度传感器 + 电机控制风扇

- #### 功能：系统持续监测温湿度（输入）。当温度超过设定阈值时，自动启动风扇（电机），并且温度越高，PWM占空比越大，风扇转速越快，实现简单的自动散热。

### **第13章：项目实战二：超声波倒车雷达模拟**

- #### 学习新的传感器：HC-SR04超声波模块

- #### 功能：测量距离，通过LED闪烁频率或蜂鸣器声音提示远近

## **附录：你的百宝箱**

- #### A. **新手救命锦囊**：最常见10个问题及解决方法

- #### B. **引脚分配速查表**：C8T6核心板常用引脚功能图

- #### C. **C语言极简速成**：只学STM32开发中用到的部分（变量、函数、判断、循环、指针、宏定义和typedef）

- #### D. **开发板原理图导读**：教你看懂原理图，找到关键元件

- #### E. **下一步学什么**：FreeRTOS，PCB设计，其他STM32系列推荐，ESP32与物联网
