---
tags:
  - SOLID
authors:
  - Yuumix
slug: csharp-solid-principles-tutorial
comments: true
---

# C# SOLID 原则

在 Unity 游戏开发中，随着项目规模的扩大，代码的可维护性、可扩展性和可测试性变得至关重要。SOLID 原则作为面向对象设计的五大基石，为我们提供了构建高质量代码的指导方针。本文将结合 Unity 开发实践和 Odin Inspector 工具，深入探讨如何在 C# 项目中应用这些原则。

<!-- more -->

## 引言：为什么需要 SOLID 原则？

### 传统 Unity 开发中的常见问题

在许多 Unity 项目中，我们经常会遇到以下问题：

1. **神类问题**：一个 MonoBehaviour 脚本承担了太多职责
2. **修改困难**：添加新功能时需要修改大量现有代码
3. **测试困难**：组件之间耦合度高，难以进行单元测试
4. **代码重复**：相似的功能在多个地方重复实现

```csharp
// ❌ 违反多个 SOLID 原则的示例
public class PlayerController : MonoBehaviour
{
    public float health = 100f;
    public float speed = 5f;
    public GameObject bulletPrefab;
    public AudioSource audioSource;
    
    void Update()
    {
        // 处理移动
        HandleMovement();
        // 处理射击
        HandleShooting();
        // 处理血量显示
        UpdateHealthUI();
        // 处理音效
        PlaySounds();
        // 处理动画
        UpdateAnimations();
    }
    
    // ... 大量混杂的功能代码
}
```

### SOLID 原则如何解决这些问题

SOLID 原则通过以下方式改善代码质量：

- **职责分离**：每个类只负责一个特定的功能
- **扩展友好**：新功能可以通过扩展而非修改现有代码来实现
- **接口规范**：通过抽象和接口降低模块间的耦合
- **依赖管理**：高层模块不直接依赖低层实现细节

## SOLID 原则概览

SOLID 是五个设计原则的首字母缩写：

| 原则  | 英文名称                        | 中文名称     | 核心理念                               |
| ----- | ------------------------------- | ------------ | -------------------------------------- |
| **S** | Single Responsibility Principle | 单一职责原则 | 一个类应该只有一个引起它变化的原因     |
| **O** | Open-Closed Principle           | 开闭原则     | 软件实体应对扩展开放，对修改封闭       |
| **L** | Liskov Substitution Principle   | 里氏替换原则 | 子类对象必须能够替换基类对象           |
| **I** | Interface Segregation Principle | 接口隔离原则 | 不应强迫客户依赖于它们不使用的接口     |
| **D** | Dependency Inversion Principle  | 依赖倒置原则 | 高层模块不应依赖低层模块，都应依赖抽象 |

这些原则相互补充，共同构建了一个健壮的软件架构基础。

## S - 单一职责原则 (Single Responsibility Principle)

### 原理阐述

单一职责原则是 SOLID 原则中最基础也是最重要的一个。它的核心思想是：**一个类应该只有一个引起它变化的原因**。换句话说，一个类应该只承担一种职责。

### 违反 SRP 的问题示例

让我们看一个典型的违反 SRP 的 Unity 脚本：

```csharp
// ❌ 违反单一职责原则的示例
public class BadPlayerController : MonoBehaviour
{
    [Header("健康系统")]
    public float maxHealth = 100f;
    private float currentHealth;
    
    [Header("移动系统")]
    public float moveSpeed = 5f;
    private Rigidbody2D rb;
    
    [Header("武器系统")]
    public GameObject bulletPrefab;
    public Transform firePoint;
    public float fireRate = 0.5f;
    private float nextFireTime;
    
    [Header("音效系统")]
    public AudioClip shootSound;
    public AudioClip hurtSound;
    private AudioSource audioSource;
    
    [Header("UI 显示")]
    public Slider healthBar;
    public Text healthText;
    
    void Start()
    {
        currentHealth = maxHealth;
        rb = GetComponent<Rigidbody2D>();
        audioSource = GetComponent<AudioSource>();
    }
    
    void Update()
    {
        HandleMovement();   // 移动职责
        HandleShooting();   // 射击职责
        UpdateHealthUI();   // UI 更新职责
        CheckHealthStatus(); // 健康状态检查职责
    }
    
    // 移动相关代码
    void HandleMovement()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        Vector2 movement = new Vector2(horizontal, vertical);
        rb.velocity = movement * moveSpeed;
    }
    
    // 射击相关代码
    void HandleShooting()
    {
        if (Input.GetButton("Fire1") && Time.time >= nextFireTime)
        {
            Shoot();
            nextFireTime = Time.time + fireRate;
        }
    }
    
    void Shoot()
    {
        Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        audioSource.PlayOneShot(shootSound);
    }
    
    // 健康系统相关代码
    public void TakeDamage(float damage)
    {
        currentHealth -= damage;
        audioSource.PlayOneShot(hurtSound);
        UpdateHealthUI();
        
        if (currentHealth <= 0)
        {
            Die();
        }
    }
    
    void Die()
    {
        // 死亡逻辑
        gameObject.SetActive(false);
    }
    
    // UI 更新相关代码
    void UpdateHealthUI()
    {
        if (healthBar != null)
            healthBar.value = currentHealth / maxHealth;
        if (healthText != null)
            healthText.text = $"{currentHealth:F0}/{maxHealth:F0}";
    }
    
    void CheckHealthStatus()
    {
        // 健康状态检查逻辑
    }
}
```

这个类违反了 SRP，因为它承担了多个职责：

- 移动控制
- 射击系统
- 健康管理
- 音效播放
- UI 更新

### 遵循 SRP 的重构方案

让我们将上述代码重构为遵循 SRP 的版本：

#### 1. 健康系统组件

```csharp
// ✅ 遵循 SRP：只负责健康管理
using Sirenix.OdinInspector;
using UnityEngine;
using UnityEngine.Events;

public class HealthComponent : MonoBehaviour
{
    [Title("健康配置")]
    [SerializeField, Range(1, 1000)] private float maxHealth = 100f;
    
    [ShowInInspector, ReadOnly, ProgressBar(0, "maxHealth", ColorGetter = "GetHealthColor")]
    private float currentHealth;
    
    [Title("事件")]
    public UnityEvent<float> OnHealthChanged;
    public UnityEvent<float> OnDamageTaken;
    public UnityEvent OnDeath;
    
    public float CurrentHealth => currentHealth;
    public float MaxHealth => maxHealth;
    public bool IsAlive => currentHealth > 0;
    public float HealthPercentage => currentHealth / maxHealth;
    
    void Start()
    {
        currentHealth = maxHealth;
    }
    
    [Button("造成伤害"), DisableInEditorMode]
    public void TakeDamage(float damage)
    {
        if (!IsAlive) return;
        
        currentHealth = Mathf.Max(0, currentHealth - damage);
        
        OnHealthChanged?.Invoke(currentHealth);
        OnDamageTaken?.Invoke(damage);
        
        if (!IsAlive)
        {
            OnDeath?.Invoke();
        }
    }
    
    [Button("恢复血量"), DisableInEditorMode]
    public void Heal(float amount)
    {
        if (!IsAlive) return;
        
        currentHealth = Mathf.Min(maxHealth, currentHealth + amount);
        OnHealthChanged?.Invoke(currentHealth);
    }
    
    // Odin Inspector 颜色获取器
    private Color GetHealthColor()
    {
        float percentage = HealthPercentage;
        if (percentage > 0.6f) return Color.green;
        if (percentage > 0.3f) return Color.yellow;
        return Color.red;
    }
}
```

#### 2. 移动控制组件

```csharp
// ✅ 遵循 SRP：只负责移动控制
using Sirenix.OdinInspector;
using UnityEngine;

public class MovementComponent : MonoBehaviour
{
    [Title("移动配置")]
    [SerializeField, Range(1, 20)] private float moveSpeed = 5f;
    
    [ShowInInspector, ReadOnly]
    private Vector2 currentVelocity;
    
    private Rigidbody2D rb;
    
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }
    
    void Update()
    {
        HandleMovementInput();
    }
    
    void HandleMovementInput()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        
        Move(new Vector2(horizontal, vertical));
    }
    
    [Button("停止移动")]
    public void Stop()
    {
        Move(Vector2.zero);
    }
    
    public void Move(Vector2 direction)
    {
        currentVelocity = direction.normalized * moveSpeed;
        rb.velocity = currentVelocity;
    }
    
    public void SetSpeed(float newSpeed)
    {
        moveSpeed = Mathf.Max(0, newSpeed);
    }
}
```

#### 3. 武器系统组件

```csharp
// ✅ 遵循 SRP：只负责武器射击
using Sirenix.OdinInspector;
using UnityEngine;
using UnityEngine.Events;

public class WeaponComponent : MonoBehaviour
{
    [Title("武器配置")]
    [SerializeField] private GameObject bulletPrefab;
    [SerializeField] private Transform firePoint;
    [SerializeField, Range(0.1f, 5f)] private float fireRate = 0.5f;
    
    [ShowInInspector, ReadOnly]
    private float nextFireTime;
    
    [Title("事件")]
    public UnityEvent OnWeaponFired;
    
    public bool CanFire => Time.time >= nextFireTime;
    
    void Update()
    {
        if (Input.GetButton("Fire1") && CanFire)
        {
            Fire();
        }
    }
    
    [Button("开火"), DisableInEditorMode]
    public void Fire()
    {
        if (!CanFire) return;
        
        Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        nextFireTime = Time.time + fireRate;
        
        OnWeaponFired?.Invoke();
    }
    
    public void SetFireRate(float newFireRate)
    {
        fireRate = Mathf.Max(0.1f, newFireRate);
    }
}
```

#### 4. 重构后的玩家控制器

```csharp
// ✅ 遵循 SRP：只负责组件协调
using Sirenix.OdinInspector;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [Title("组件引用")]
    [SerializeField, Required] private HealthComponent healthComponent;
    [SerializeField, Required] private MovementComponent movementComponent;
    [SerializeField, Required] private WeaponComponent weaponComponent;
    [SerializeField, Required] private AudioSource audioSource;
    
    [Title("音效配置")]
    [SerializeField] private AudioClip shootSound;
    [SerializeField] private AudioClip hurtSound;
    
    void Start()
    {
        SetupEventListeners();
    }
    
    void SetupEventListeners()
    {
        // 监听健康变化事件
        healthComponent.OnDamageTaken.AddListener(OnPlayerHurt);
        healthComponent.OnDeath.AddListener(OnPlayerDeath);
        
        // 监听武器射击事件
        weaponComponent.OnWeaponFired.AddListener(OnWeaponFired);
    }
    
    void OnPlayerHurt(float damage)
    {
        if (hurtSound != null)
            audioSource.PlayOneShot(hurtSound);
    }
    
    void OnPlayerDeath()
    {
        // 处理玩家死亡
        movementComponent.Stop();
        // 可以触发游戏结束逻辑
    }
    
    void OnWeaponFired()
    {
        if (shootSound != null)
            audioSource.PlayOneShot(shootSound);
    }
    
    void OnDestroy()
    {
        // 清理事件监听
        if (healthComponent != null)
        {
            healthComponent.OnDamageTaken.RemoveListener(OnPlayerHurt);
            healthComponent.OnDeath.RemoveListener(OnPlayerDeath);
        }
        
        if (weaponComponent != null)
        {
            weaponComponent.OnWeaponFired.RemoveListener(OnWeaponFired);
        }
    }
}
```

### SRP 的优势

重构后的代码具有以下优势：

1. **职责清晰**：每个组件只负责一个特定功能
2. **易于测试**：可以单独测试每个组件
3. **易于维护**：修改某个功能不会影响其他功能
4. **可重用性高**：组件可以在其他地方复用
5. **易于扩展**：添加新功能只需要添加新组件

!!! tip "Odin Inspector 的 SRP 支持"
    - 使用 `[Title]` 和 `[BoxGroup]` 来组织单一职责的配置项
    - 使用 `[Required]` 特性确保必要依赖的完整性
    - 使用 `[Button]` 特性提供测试接口
    - 使用 `[ShowInInspector]` 和 `[ReadOnly]` 显示运行时状态

## O - 开闭原则 (Open-Closed Principle)

### 原理阐述

开闭原则要求软件实体（类、模块、函数等）**对扩展开放，对修改封闭**。这意味着当需要添加新功能时，应该通过扩展现有代码来实现，而不是修改现有代码。

### 违反 OCP 的问题示例

考虑一个武器系统，如果不遵循开闭原则：

```csharp
// ❌ 违反开闭原则的武器系统
public enum WeaponType
{
    Pistol,
    Rifle,
    Shotgun
    // 每次添加新武器都需要修改这个枚举
}

public class BadWeaponSystem : MonoBehaviour
{
    public WeaponType weaponType;
    public GameObject bulletPrefab;
    public Transform firePoint;
    
    public void Fire()
    {
        // 每次添加新武器都需要修改这个方法
        switch (weaponType)
        {
            case WeaponType.Pistol:
                FirePistol();
                break;
            case WeaponType.Rifle:
                FireRifle();
                break;
            case WeaponType.Shotgun:
                FireShotgun();
                break;
            // 需要不断添加新的 case
        }
    }
    
    void FirePistol()
    {
        // 手枪射击逻辑
        Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
    }
    
    void FireRifle()
    {
        // 步枪射击逻辑（三连发）
        for (int i = 0; i < 3; i++)
        {
            Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        }
    }
    
    void FireShotgun()
    {
        // 鸳弹枪射击逻辑（散弹）
        for (int i = 0; i < 5; i++)
        {
            float angle = (i - 2) * 15f;
            Quaternion rotation = Quaternion.Euler(0, 0, angle) * firePoint.rotation;
            Instantiate(bulletPrefab, firePoint.position, rotation);
        }
    }
}
```

这种设计的问题：

- 每次添加新武器都需要修改现有代码
- 代码修改可能引入新的 Bug
- 难以进行单元测试
- 不符合开闭原则

### 遵循 OCP 的重构方案

使用策略模式和接口来遵循开闭原则：

#### 1. 定义武器接口

```csharp
// ✅ 武器接口：对扩展开放
using UnityEngine;

public interface IWeapon
{
    string WeaponName { get; }
    float Damage { get; }
    float FireRate { get; }
    void Fire(Transform firePoint, GameObject bulletPrefab);
    bool CanFire();
}
```

#### 2. 具体武器实现

```csharp
// ✅ 手枪实现
using Sirenix.OdinInspector;
using UnityEngine;

[System.Serializable]
public class PistolWeapon : IWeapon
{
    [Title("手枪配置")]
    [SerializeField] private string weaponName = "手枪";
    [SerializeField, Range(10, 50)] private float damage = 25f;
    [SerializeField, Range(0.1f, 2f)] private float fireRate = 0.5f;
    
    private float lastFireTime;
    
    public string WeaponName => weaponName;
    public float Damage => damage;
    public float FireRate => fireRate;
    
    public bool CanFire()
    {
        return Time.time >= lastFireTime + fireRate;
    }
    
    public void Fire(Transform firePoint, GameObject bulletPrefab)
    {
        if (!CanFire()) return;
        
        GameObject bullet = Object.Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        if (bullet.TryGetComponent<Bullet>(out var bulletComponent))
        {
            bulletComponent.SetDamage(damage);
        }
        
        lastFireTime = Time.time;
    }
}

// ✅ 步枪实现
[System.Serializable]
public class RifleWeapon : IWeapon
{
    [Title("步枪配置")]
    [SerializeField] private string weaponName = "步枪";
    [SerializeField, Range(15, 40)] private float damage = 20f;
    [SerializeField, Range(0.05f, 0.3f)] private float fireRate = 0.1f;
    [SerializeField, Range(1, 5)] private int burstCount = 3;
    
    private float lastFireTime;
    
    public string WeaponName => weaponName;
    public float Damage => damage;
    public float FireRate => fireRate;
    
    public bool CanFire()
    {
        return Time.time >= lastFireTime + fireRate;
    }
    
    public void Fire(Transform firePoint, GameObject bulletPrefab)
    {
        if (!CanFire()) return;
        
        // 三连发射击
        for (int i = 0; i < burstCount; i++)
        {
            GameObject bullet = Object.Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
            if (bullet.TryGetComponent<Bullet>(out var bulletComponent))
            {
                bulletComponent.SetDamage(damage);
            }
        }
        
        lastFireTime = Time.time;
    }
}

// ✅ 鸳弹枪实现
[System.Serializable]
public class ShotgunWeapon : IWeapon
{
    [Title("鸳弹枪配置")]
    [SerializeField] private string weaponName = "鸳弹枪";
    [SerializeField, Range(5, 15)] private float damage = 8f;
    [SerializeField, Range(0.5f, 3f)] private float fireRate = 1.2f;
    [SerializeField, Range(3, 8)] private int pelletCount = 5;
    [SerializeField, Range(5, 30)] private float spreadAngle = 15f;
    
    private float lastFireTime;
    
    public string WeaponName => weaponName;
    public float Damage => damage;
    public float FireRate => fireRate;
    
    public bool CanFire()
    {
        return Time.time >= lastFireTime + fireRate;
    }
    
    public void Fire(Transform firePoint, GameObject bulletPrefab)
    {
        if (!CanFire()) return;
        
        // 散弹射击
        for (int i = 0; i < pelletCount; i++)
        {
            float angle = (i - (pelletCount - 1) / 2f) * spreadAngle;
            Quaternion rotation = Quaternion.Euler(0, 0, angle) * firePoint.rotation;
            
            GameObject bullet = Object.Instantiate(bulletPrefab, firePoint.position, rotation);
            if (bullet.TryGetComponent<Bullet>(out var bulletComponent))
            {
                bulletComponent.SetDamage(damage);
            }
        }
        
        lastFireTime = Time.time;
    }
}
```

#### 3. 武器系统控制器

```csharp
// ✅ 遵循开闭原则的武器系统
using Sirenix.OdinInspector;
using UnityEngine;
using UnityEngine.Events;

public class WeaponSystem : MonoBehaviour
{
    [Title("武器配置")]
    [SerializeReference, Sirenix.OdinInspector.LabelText("当前武器")]
    private IWeapon currentWeapon;
    
    [SerializeField, Required] private Transform firePoint;
    [SerializeField, Required] private GameObject bulletPrefab;
    
    [Title("预设武器")]
    [SerializeField] private PistolWeapon pistol = new PistolWeapon();
    [SerializeField] private RifleWeapon rifle = new RifleWeapon();
    [SerializeField] private ShotgunWeapon shotgun = new ShotgunWeapon();
    
    [Title("事件")]
    public UnityEvent<string> OnWeaponChanged;
    public UnityEvent OnWeaponFired;
    
    [ShowInInspector, ReadOnly]
    public string CurrentWeaponName => currentWeapon?.WeaponName ?? "无";
    
    void Start()
    {
        // 默认武器
        EquipWeapon(pistol);
    }
    
    void Update()
    {
        HandleInput();
    }
    
    void HandleInput()
    {
        // 射击
        if (Input.GetButton("Fire1"))
        {
            Fire();
        }
        
        // 切换武器
        if (Input.GetKeyDown(KeyCode.Alpha1)) EquipWeapon(pistol);
        if (Input.GetKeyDown(KeyCode.Alpha2)) EquipWeapon(rifle);
        if (Input.GetKeyDown(KeyCode.Alpha3)) EquipWeapon(shotgun);
    }
    
    [Button("开火"), DisableInEditorMode]
    public void Fire()
    {
        if (currentWeapon != null && currentWeapon.CanFire())
        {
            currentWeapon.Fire(firePoint, bulletPrefab);
            OnWeaponFired?.Invoke();
        }
    }
    
    [Button("装备手枪")]
    public void EquipPistol() => EquipWeapon(pistol);
    
    [Button("装备步枪")]
    public void EquipRifle() => EquipWeapon(rifle);
    
    [Button("装备鸳弹枪")]
    public void EquipShotgun() => EquipWeapon(shotgun);
    
    public void EquipWeapon(IWeapon weapon)
    {
        currentWeapon = weapon;
        OnWeaponChanged?.Invoke(weapon.WeaponName);
    }
}
```

### 添加新武器：对扩展开放

现在添加一个新的激光枪，无需修改现有代码：

```csharp
// ✅ 新武器：激光枪（无需修改现有代码）
[System.Serializable]
public class LaserWeapon : IWeapon
{
    [Title("激光枪配置")]
    [SerializeField] private string weaponName = "激光枪";
    [SerializeField, Range(30, 100)] private float damage = 50f;
    [SerializeField, Range(0.3f, 1.5f)] private float fireRate = 0.8f;
    [SerializeField, Range(5, 20)] private float laserDuration = 2f;
    
    private float lastFireTime;
    
    public string WeaponName => weaponName;
    public float Damage => damage;
    public float FireRate => fireRate;
    
    public bool CanFire()
    {
        return Time.time >= lastFireTime + fireRate;
    }
    
    public void Fire(Transform firePoint, GameObject bulletPrefab)
    {
        if (!CanFire()) return;
        
        // 创建激光束效果
        GameObject laser = Object.Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
        if (laser.TryGetComponent<LaserBeam>(out var laserComponent))
        {
            laserComponent.SetDamage(damage);
            laserComponent.SetDuration(laserDuration);
        }
        
        lastFireTime = Time.time;
    }
}
```

只需要在 `WeaponSystem` 中添加一个字段：

```csharp
[SerializeField] private LaserWeapon laser = new LaserWeapon();

[Button("装备激光枪")]
public void EquipLaser() => EquipWeapon(laser);
```

### OCP 的优势

1. **扩展性**：添加新功能无需修改现有代码
2. **数据稳定性**：现有功能不会被新功能影响
3. **测试友好**：每个武器可以单独测试
4. **代码复用**：武器实现可以在其他系统中复用

!!! tip "Odin Inspector 的 OCP 支持"
    - 使用 `[SerializeReference]` 实现接口序列化
    - 使用 `[Button]` 提供运行时切换功能
    - 使用 `[Title]` 和 `[BoxGroup]` 组织不同的武器配置
    - 使用 `[ShowInInspector]` 显示当前状态

## L - 里氏替换原则 (Liskov Substitution Principle)

### 原理阐述

里氏替换原则要求：**子类对象必须能够替换其基类对象，而不会影响程序的正确性**。这意味着子类应该增强而不是删弱基类的功能。

### 违反 LSP 的问题示例

考虑一个飞行物体的继承体系：

```csharp
// ❌ 违反里氏替换原则的设计
public abstract class FlyingObject : MonoBehaviour
{
    public float speed = 10f;
    
    public virtual void Fly()
    {
        transform.Translate(Vector3.forward * speed * Time.deltaTime);
    }
    
    public virtual void Land()
    {
        // 默认着陆行为
        speed = 0;
    }
}

public class Airplane : FlyingObject
{
    public override void Fly()
    {
        // 飞机正常飞行
        transform.Translate(Vector3.forward * speed * Time.deltaTime);
    }
    
    public override void Land()
    {
        // 飞机可以着陆
        speed = 0;
        Debug.Log("飞机已着陆");
    }
}

// 问题：企鹅不能飞行，违反了 LSP
public class Penguin : FlyingObject
{
    public override void Fly()
    {
        // 企鹅不能飞！
        throw new System.NotSupportedException("企鹅不能飞行！");
    }
    
    public override void Land()
    {
        // 企鹅本来就在地上，这个方法没有意义
        Debug.Log("企鹅本来就在地上...");
    }
}

// 使用时会出现问题
public class FlightController : MonoBehaviour
{
    public FlyingObject[] flyingObjects;
    
    void Start()
    {
        foreach (var obj in flyingObjects)
        {
            obj.Fly(); // 如果数组中有企鹅，这里会抛出异常！
        }
    }
}
```

### 遵循 LSP 的重构方案

重新设计类层次结构，使其遵循里氏替换原则：

#### 1. 重新设计基类结构

```csharp
// ✅ 遵循 LSP 的设计：动物基类
using Sirenix.OdinInspector;
using UnityEngine;

public abstract class Animal : MonoBehaviour
{
    [Title("动物基本信息")]
    [SerializeField, ReadOnly] protected string animalName;
    [SerializeField, Range(1, 50)] protected float moveSpeed = 5f;
    
    [ShowInInspector, ReadOnly]
    public string AnimalName => animalName;
    
    [ShowInInspector, ReadOnly]
    public float MoveSpeed => moveSpeed;
    
    protected virtual void Start()
    {
        animalName = GetType().Name;
    }
    
    // 所有动物都可以移动
    public abstract void Move(Vector3 direction);
    
    // 所有动物都可以发出声音
    public abstract void MakeSound();
}

// ✅ 可飞行动物接口
public interface IFlyable
{
    float FlySpeed { get; }
    void Fly(Vector3 direction);
    void Land();
    bool IsFlying { get; }
}

// ✅ 可游泳动物接口
public interface ISwimmable
{
    float SwimSpeed { get; }
    void Swim(Vector3 direction);
    bool IsSwimming { get; }
}
```

#### 2. 具体动物实现

```csharp
// ✅ 企鹅：只实现它能做的事情
using Sirenix.OdinInspector;
using UnityEngine;

public class Penguin : Animal, ISwimmable
{
    [Title("企鹅特有属性")]
    [SerializeField, Range(1, 20)] private float swimSpeed = 8f;
    [SerializeField] private bool isSwimming = false;
    
    public float SwimSpeed => swimSpeed;
    public bool IsSwimming => isSwimming;
    
    public override void Move(Vector3 direction)
    {
        // 企鹅在陆地上走路
        transform.Translate(direction.normalized * moveSpeed * Time.deltaTime);
        Debug.Log($"{animalName} 在陆地上行走");
    }
    
    public void Swim(Vector3 direction)
    {
        isSwimming = true;
        transform.Translate(direction.normalized * swimSpeed * Time.deltaTime);
        Debug.Log($"{animalName} 在水中游泳");
    }
    
    public override void MakeSound()
    {
        Debug.Log("企鹅叫：噖噖噖！");
    }
    
    [Button("开始/停止游泳")]
    public void ToggleSwimming()
    {
        isSwimming = !isSwimming;
    }
}

// ✅ 鹰：实现飞行能力
public class Eagle : Animal, IFlyable
{
    [Title("鹰特有属性")]
    [SerializeField, Range(5, 30)] private float flySpeed = 15f;
    [SerializeField] private bool isFlying = false;
    
    public float FlySpeed => flySpeed;
    public bool IsFlying => isFlying;
    
    public override void Move(Vector3 direction)
    {
        if (isFlying)
        {
            Fly(direction);
        }
        else
        {
            // 在地面行走
            transform.Translate(direction.normalized * moveSpeed * Time.deltaTime);
            Debug.Log($"{animalName} 在地面行走");
        }
    }
    
    public void Fly(Vector3 direction)
    {
        isFlying = true;
        transform.Translate(direction.normalized * flySpeed * Time.deltaTime);
        Debug.Log($"{animalName} 在天空飞翔");
    }
    
    public void Land()
    {
        isFlying = false;
        Debug.Log($"{animalName} 已着陆");
    }
    
    public override void MakeSound()
    {
        Debug.Log("鹰叫：嘛嘛嘛！");
    }
    
    [Button("起飞/着陆")]
    public void ToggleFlight()
    {
        if (isFlying)
            Land();
        else
            isFlying = true;
    }
}

// ✅ 鸭子：实现多种能力
public class Duck : Animal, IFlyable, ISwimmable
{
    [Title("鸭子特有属性")]
    [SerializeField, Range(3, 15)] private float flySpeed = 10f;
    [SerializeField, Range(2, 12)] private float swimSpeed = 6f;
    [SerializeField] private bool isFlying = false;
    [SerializeField] private bool isSwimming = false;
    
    public float FlySpeed => flySpeed;
    public float SwimSpeed => swimSpeed;
    public bool IsFlying => isFlying;
    public bool IsSwimming => isSwimming;
    
    public override void Move(Vector3 direction)
    {
        if (isFlying)
        {
            Fly(direction);
        }
        else if (isSwimming)
        {
            Swim(direction);
        }
        else
        {
            // 在陆地行走
            transform.Translate(direction.normalized * moveSpeed * Time.deltaTime);
            Debug.Log($"{animalName} 在陆地行走");
        }
    }
    
    public void Fly(Vector3 direction)
    {
        isFlying = true;
        isSwimming = false;
        transform.Translate(direction.normalized * flySpeed * Time.deltaTime);
        Debug.Log($"{animalName} 在天空飞翔");
    }
    
    public void Land()
    {
        isFlying = false;
        Debug.Log($"{animalName} 已着陆");
    }
    
    public void Swim(Vector3 direction)
    {
        isSwimming = true;
        isFlying = false;
        transform.Translate(direction.normalized * swimSpeed * Time.deltaTime);
        Debug.Log($"{animalName} 在水中游泳");
    }
    
    public override void MakeSound()
    {
        Debug.Log("鸭子叫：嘉嘉嘉！");
    }
    
    [Button("起飞/着陆")]
    public void ToggleFlight()
    {
        if (isFlying)
            Land();
        else
        {
            isFlying = true;
            isSwimming = false;
        }
    }
    
    [Button("开始/停止游泳")]
    public void ToggleSwimming()
    {
        isSwimming = !isSwimming;
        if (isSwimming)
            isFlying = false;
    }
}
```

#### 3. 遵循 LSP 的控制器

```csharp
// ✅ 遵循 LSP 的控制器
using Sirenix.OdinInspector;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;

public class AnimalController : MonoBehaviour
{
    [Title("动物集合")]
    [SerializeField] private Animal[] allAnimals;
    
    [ShowInInspector, ReadOnly]
    private List<IFlyable> flyableAnimals = new List<IFlyable>();
    
    [ShowInInspector, ReadOnly]
    private List<ISwimmable> swimmableAnimals = new List<ISwimmable>();
    
    void Start()
    {
        CategorizeAnimals();
    }
    
    void CategorizeAnimals()
    {
        flyableAnimals.Clear();
        swimmableAnimals.Clear();
        
        foreach (var animal in allAnimals)
        {
            if (animal is IFlyable flyable)
                flyableAnimals.Add(flyable);
                
            if (animal is ISwimmable swimmable)
                swimmableAnimals.Add(swimmable);
        }
    }
    
    [Button("所有动物移动")]
    public void MoveAllAnimals()
    {
        foreach (var animal in allAnimals)
        {
            // 所有动物都能移动，符合 LSP
            animal.Move(Vector3.forward);
        }
    }
    
    [Button("所有动物发声")]
    public void MakeAllAnimalsSound()
    {
        foreach (var animal in allAnimals)
        {
            // 所有动物都能发声，符合 LSP
            animal.MakeSound();
        }
    }
    
    [Button("所有会飞的动物飞行")]
    public void MakeAllFlyableAnimalsFly()
    {
        foreach (var flyable in flyableAnimals)
        {
            flyable.Fly(Vector3.up);
        }
    }
    
    [Button("所有会游泳的动物游泳")]
    public void MakeAllSwimmableAnimalsSwim()
    {
        foreach (var swimmable in swimmableAnimals)
        {
            swimmable.Swim(Vector3.forward);
        }
    }
}
```

### LSP 的关键原则

1. **子类必须能替换基类**：在任何使用基类的地方，都可以使用子类
2. **不能弱化基类的功能**：子类不能抛出基类不会抛出的异常
3. **保持行为一致性**：子类的行为应该与基类的期望一致
4. **子类可以增强功能**：但不能删弱原有功能

!!! tip "Odin Inspector 的 LSP 支持"
    - 使用 `[ShowInInspector]` 显示类型信息和状态
    - 使用 `[Button]` 提供不同行为的测试接口
    - 使用 `[ReadOnly]` 防止意外修改关键属性
    - 使用 `[Title]` 清晰区分不同类型的功能

## I - 接口隔离原则 (Interface Segregation Principle)

### 原理阐述

接口隔离原则要求：**不应该强迫客户端依赖于它们不使用的接口**。接口应该细粒度且专门针对特定的客户端需求。

### 违反 ISP 的问题示例

考虑一个臃胖的游戏对象接口：

```csharp
// ❌ 违反接口隔离原则的臃胖接口
public interface IBloatedGameObject
{
    // 移动相关
    void Move(Vector3 direction);
    void Stop();
    float GetSpeed();
    
    // 战斗相关
    void Attack(GameObject target);
    void TakeDamage(float damage);
    float GetHealth();
    
    // 道具相关
    void UseItem(int itemId);
    void PickupItem(GameObject item);
    void DropItem(int itemId);
    
    // NPC 对话相关
    void StartDialogue();
    void EndDialogue();
    string GetDialogueText();
    
    // 商店相关
    void OpenShop();
    void BuyItem(int itemId);
    void SellItem(int itemId);
    
    // 任务相关
    void AcceptQuest(int questId);
    void CompleteQuest(int questId);
    void GetQuestProgress(int questId);
}

// 问题：武器物品不需要对话和商店功能
public class Weapon : MonoBehaviour, IBloatedGameObject
{
    public void Move(Vector3 direction) { /* 正常实现 */ }
    public void Stop() { /* 正常实现 */ }
    public float GetSpeed() { return 0; }
    
    public void Attack(GameObject target) { /* 正常实现 */ }
    public void TakeDamage(float damage) { /* 正常实现 */ }
    public float GetHealth() { return 100; }
    
    // 不需要的方法，但被迫实现
    public void UseItem(int itemId) { throw new System.NotImplementedException(); }
    public void PickupItem(GameObject item) { throw new System.NotImplementedException(); }
    public void DropItem(int itemId) { throw new System.NotImplementedException(); }
    
    public void StartDialogue() { throw new System.NotImplementedException(); }
    public void EndDialogue() { throw new System.NotImplementedException(); }
    public string GetDialogueText() { throw new System.NotImplementedException(); }
    
    public void OpenShop() { throw new System.NotImplementedException(); }
    public void BuyItem(int itemId) { throw new System.NotImplementedException(); }
    public void SellItem(int itemId) { throw new System.NotImplementedException(); }
    
    public void AcceptQuest(int questId) { throw new System.NotImplementedException(); }
    public void CompleteQuest(int questId) { throw new System.NotImplementedException(); }
    public void GetQuestProgress(int questId) { throw new System.NotImplementedException(); }
}
```

### 遵循 ISP 的重构方案

将臃胖接口拆分为多个细粒度的接口：

#### 1. 细粒度接口定义

```csharp
// ✅ 移动能力接口
public interface IMoveable
{
    void Move(Vector3 direction);
    void Stop();
    float Speed { get; set; }
}

// ✅ 战斗能力接口
public interface ICombatable
{
    void Attack(GameObject target);
    void TakeDamage(float damage);
    float Health { get; }
    float MaxHealth { get; }
    bool IsAlive { get; }
}

// ✅ 道具使用接口
public interface IItemUser
{
    void UseItem(int itemId);
    void PickupItem(GameObject item);
    void DropItem(int itemId);
    int[] GetInventory();
}

// ✅ 对话能力接口
public interface IDialogable
{
    void StartDialogue();
    void EndDialogue();
    string GetDialogueText();
    bool IsInDialogue { get; }
}

// ✅ 商店接口
public interface IShopkeeper
{
    void OpenShop();
    void BuyItem(int itemId, int quantity);
    void SellItem(int itemId, int quantity);
    bool HasItem(int itemId);
    int GetItemPrice(int itemId);
}

// ✅ 任务系统接口
public interface IQuestGiver
{
    void GiveQuest(int questId);
    bool CanGiveQuest(int questId);
}

public interface IQuestReceiver
{
    void AcceptQuest(int questId);
    void CompleteQuest(int questId);
    float GetQuestProgress(int questId);
}
```

#### 2. 精简的实现类

```csharp
// ✅ 武器：只实现需要的接口
using Sirenix.OdinInspector;
using UnityEngine;

public class Weapon : MonoBehaviour, IMoveable, ICombatable
{
    [Title("武器属性")]
    [SerializeField, Range(1, 20)] private float speed = 5f;
    [SerializeField, Range(10, 100)] private float health = 50f;
    [SerializeField, Range(10, 100)] private float maxHealth = 50f;
    [SerializeField, Range(10, 50)] private float damage = 25f;
    
    public float Speed 
    { 
        get => speed; 
        set => speed = Mathf.Max(0, value); 
    }
    
    public float Health => health;
    public float MaxHealth => maxHealth;
    public bool IsAlive => health > 0;
    
    public void Move(Vector3 direction)
    {
        transform.Translate(direction.normalized * speed * Time.deltaTime);
    }
    
    public void Stop()
    {
        // 停止移动逻辑
    }
    
    public void Attack(GameObject target)
    {
        if (target.TryGetComponent<ICombatable>(out var combatable))
        {
            combatable.TakeDamage(damage);
        }
    }
    
    public void TakeDamage(float damage)
    {
        health = Mathf.Max(0, health - damage);
        if (!IsAlive)
        {
            Destroy(gameObject);
        }
    }
}

// ✅ NPC：实现多个相关接口
public class NPC : MonoBehaviour, IMoveable, ICombatable, IDialogable, IShopkeeper
{
    [Title("NPC 基本属性")]
    [SerializeField, Range(1, 10)] private float speed = 3f;
    [SerializeField, Range(50, 200)] private float health = 100f;
    [SerializeField, Range(50, 200)] private float maxHealth = 100f;
    
    [Title("对话系统")]
    [SerializeField] private string[] dialogueLines;
    [SerializeField] private bool isInDialogue = false;
    
    [Title("商店系统")]
    [SerializeField] private int[] shopItems;
    [SerializeField] private int[] itemPrices;
    
    // IMoveable 实现
    public float Speed 
    { 
        get => speed; 
        set => speed = Mathf.Max(0, value); 
    }
    
    public void Move(Vector3 direction)
    {
        if (!isInDialogue)
            transform.Translate(direction.normalized * speed * Time.deltaTime);
    }
    
    public void Stop() { }
    
    // ICombatable 实现
    public float Health => health;
    public float MaxHealth => maxHealth;
    public bool IsAlive => health > 0;
    
    public void Attack(GameObject target)
    {
        // NPC 攻击逻辑
    }
    
    public void TakeDamage(float damage)
    {
        health = Mathf.Max(0, health - damage);
    }
    
    // IDialogable 实现
    public bool IsInDialogue => isInDialogue;
    
    public void StartDialogue()
    {
        isInDialogue = true;
        Stop();
    }
    
    public void EndDialogue()
    {
        isInDialogue = false;
    }
    
    public string GetDialogueText()
    {
        return dialogueLines[Random.Range(0, dialogueLines.Length)];
    }
    
    // IShopkeeper 实现
    public void OpenShop()
    {
        Debug.Log("NPC 商店开启");
    }
    
    public void BuyItem(int itemId, int quantity)
    {
        // 购买逻辑
    }
    
    public void SellItem(int itemId, int quantity)
    {
        // 出售逻辑
    }
    
    public bool HasItem(int itemId)
    {
        return System.Array.Exists(shopItems, item => item == itemId);
    }
    
    public int GetItemPrice(int itemId)
    {
        int index = System.Array.IndexOf(shopItems, itemId);
        return index >= 0 ? itemPrices[index] : 0;
    }
}

// ✅ 玩家：实现与玩家相关的接口
public class Player : MonoBehaviour, IMoveable, ICombatable, IItemUser, IQuestReceiver
{
    [Title("玩家属性")]
    [SerializeField] private float speed = 8f;
    [SerializeField] private float health = 100f;
    [SerializeField] private float maxHealth = 100f;
    
    [Title("道具系统")]
    [SerializeField] private int[] inventory = new int[10];
    
    [Title("任务系统")]
    [SerializeField] private int[] activeQuests = new int[5];
    
    // 实现所有需要的接口方法...
    public float Speed { get => speed; set => speed = value; }
    public float Health => health;
    public float MaxHealth => maxHealth;
    public bool IsAlive => health > 0;
    
    public void Move(Vector3 direction)
    {
        transform.Translate(direction.normalized * speed * Time.deltaTime);
    }
    
    public void Stop() { }
    
    public void Attack(GameObject target) 
    {
        // 玩家攻击逻辑
    }
    
    public void TakeDamage(float damage)
    {
        health = Mathf.Max(0, health - damage);
    }
    
    public void UseItem(int itemId) { /* 使用道具 */ }
    public void PickupItem(GameObject item) { /* 拾取道具 */ }
    public void DropItem(int itemId) { /* 丢弃道具 */ }
    public int[] GetInventory() => inventory;
    
    public void AcceptQuest(int questId) { /* 接受任务 */ }
    public void CompleteQuest(int questId) { /* 完成任务 */ }
    public float GetQuestProgress(int questId) { return 0f; }
}
```

### ISP 的优势

1. **接口精简**：每个接口只包含相关的方法
2. **降低耦合**：类只依赖于它们实际使用的接口
3. **易于维护**：修改一个接口不会影响不相关的类
4. **提高单元测试效率**：可以单独模拟每个细粒度接口

!!! tip "Odin Inspector 的 ISP 支持"
    - 使用 `[Title]` 将不同接口的功能分组显示
    - 使用 `[ShowInInspector]` 适度暴露接口状态
    - 使用 `[Required]` 确保接口依赖的完整性
    - 使用 `[Button]` 为每个接口提供测试入口

## D - 依赖倒置原则 (Dependency Inversion Principle)

### 原理阐述

依赖倒置原则包含两个重要概念：

1. **高层模块不应该依赖于低层模块，两者都应该依赖于抽象**
2. **抽象不应该依赖于实现，实现应该依赖于抽象**

这个原则的核心是通过抽象（接口或抽象类）来降低模块间的耦合度。

### 违反 DIP 的问题示例

考虑一个游戏保存系统：

```csharp
// ❌ 违反依赖倒置原则的设计
public class FileDataStorage
{
    public void SaveData(string data, string filename)
    {
        System.IO.File.WriteAllText(filename, data);
        Debug.Log("数据已保存到文件: " + filename);
    }
    
    public string LoadData(string filename)
    {
        if (System.IO.File.Exists(filename))
        {
            return System.IO.File.ReadAllText(filename);
        }
        return string.Empty;
    }
}

// 问题：高层的 GameManager 直接依赖于低层的 FileDataStorage
public class BadGameManager : MonoBehaviour
{
    private FileDataStorage fileStorage; // 直接依赖具体实现
    
    void Start()
    {
        fileStorage = new FileDataStorage(); // 紧耦合
    }
    
    public void SaveGame()
    {
        string gameData = GetGameData();
        fileStorage.SaveData(gameData, "savegame.json"); // 直接调用具体实现
    }
    
    public void LoadGame()
    {
        string gameData = fileStorage.LoadData("savegame.json");
        ApplyGameData(gameData);
    }
    
    // 要更换为云存储，必须修改这个类
    // 要支持多种存储方式，需要大量修改
    
    private string GetGameData() { return "{}"; }
    private void ApplyGameData(string data) { }
}
```

这种设计的问题：

- `GameManager`（高层）直接依赖于 `FileDataStorage`（低层）
- 难以扩展为云存储或其他存储方式
- 难以进行单元测试（无法模拟存储）

### 遵循 DIP 的重构方案

通过抽象接口和依赖注入来解决问题：

#### 1. 定义抽象接口

```csharp
// ✅ 存储系统抽象接口
public interface IDataStorage
{
    void SaveData(string data, string key);
    string LoadData(string key);
    bool HasData(string key);
    void DeleteData(string key);
}

// ✅ 事件系统抽象接口
public interface IEventSystem
{
    void Subscribe<T>(System.Action<T> handler) where T : class;
    void Unsubscribe<T>(System.Action<T> handler) where T : class;
    void Publish<T>(T eventData) where T : class;
}

// ✅ 日志系统抽象接口
public interface ILogger
{
    void Log(string message);
    void LogWarning(string message);
    void LogError(string message);
}
```

#### 2. 具体实现（低层模块）

```csharp
// ✅ 文件存储实现
using Sirenix.OdinInspector;
using UnityEngine;
using System.IO;

public class FileDataStorage : IDataStorage
{
    [Title("文件存储配置")]
    [SerializeField, FolderPath] private string saveDirectory = "SaveData";
    [SerializeField] private string fileExtension = ".json";
    
    public void SaveData(string data, string key)
    {
        string fullPath = GetFullPath(key);
        Directory.CreateDirectory(Path.GetDirectoryName(fullPath));
        File.WriteAllText(fullPath, data);
        Debug.Log($"数据已保存到: {fullPath}");
    }
    
    public string LoadData(string key)
    {
        string fullPath = GetFullPath(key);
        if (File.Exists(fullPath))
        {
            return File.ReadAllText(fullPath);
        }
        Debug.LogWarning($"文件不存在: {fullPath}");
        return string.Empty;
    }
    
    public bool HasData(string key)
    {
        return File.Exists(GetFullPath(key));
    }
    
    public void DeleteData(string key)
    {
        string fullPath = GetFullPath(key);
        if (File.Exists(fullPath))
        {
            File.Delete(fullPath);
            Debug.Log($"文件已删除: {fullPath}");
        }
    }
    
    private string GetFullPath(string key)
    {
        return Path.Combine(Application.persistentDataPath, saveDirectory, key + fileExtension);
    }
}

// ✅ PlayerPrefs 存储实现
public class PlayerPrefsDataStorage : IDataStorage
{
    public void SaveData(string data, string key)
    {
        PlayerPrefs.SetString(key, data);
        PlayerPrefs.Save();
        Debug.Log($"数据已保存到 PlayerPrefs: {key}");
    }
    
    public string LoadData(string key)
    {
        return PlayerPrefs.GetString(key, string.Empty);
    }
    
    public bool HasData(string key)
    {
        return PlayerPrefs.HasKey(key);
    }
    
    public void DeleteData(string key)
    {
        PlayerPrefs.DeleteKey(key);
        PlayerPrefs.Save();
        Debug.Log($"PlayerPrefs 键已删除: {key}");
    }
}

// ✅ Unity 日志实现
public class UnityLogger : ILogger
{
    public void Log(string message)
    {
        Debug.Log($"[INFO] {message}");
    }
    
    public void LogWarning(string message)
    {
        Debug.LogWarning($"[WARNING] {message}");
    }
    
    public void LogError(string message)
    {
        Debug.LogError($"[ERROR] {message}");
    }
}

// ✅ 简单事件系统实现
using System.Collections.Generic;
using System;

public class SimpleEventSystem : IEventSystem
{
    private Dictionary<Type, List<Delegate>> eventHandlers = new Dictionary<Type, List<Delegate>>();
    
    public void Subscribe<T>(Action<T> handler) where T : class
    {
        Type eventType = typeof(T);
        if (!eventHandlers.ContainsKey(eventType))
        {
            eventHandlers[eventType] = new List<Delegate>();
        }
        eventHandlers[eventType].Add(handler);
    }
    
    public void Unsubscribe<T>(Action<T> handler) where T : class
    {
        Type eventType = typeof(T);
        if (eventHandlers.ContainsKey(eventType))
        {
            eventHandlers[eventType].Remove(handler);
        }
    }
    
    public void Publish<T>(T eventData) where T : class
    {
        Type eventType = typeof(T);
        if (eventHandlers.ContainsKey(eventType))
        {
            foreach (var handler in eventHandlers[eventType])
            {
                (handler as Action<T>)?.Invoke(eventData);
            }
        }
    }
}
```

#### 3. 依赖注入容器

```csharp
// ✅ 简单的依赖注入容器
using System;
using System.Collections.Generic;
using Sirenix.OdinInspector;
using UnityEngine;

public class ServiceContainer : MonoBehaviour
{
    private static ServiceContainer instance;
    public static ServiceContainer Instance 
    { 
        get
        {
            if (instance == null)
            {
                instance = FindObjectOfType<ServiceContainer>();
                if (instance == null)
                {
                    GameObject go = new GameObject("ServiceContainer");
                    instance = go.AddComponent<ServiceContainer>();
                    DontDestroyOnLoad(go);
                }
            }
            return instance;
        }
    }
    
    [Title("服务配置")]
    [SerializeField] private bool useFileStorage = true;
    [SerializeField] private bool useUnityLogger = true;
    
    private Dictionary<Type, object> services = new Dictionary<Type, object>();
    
    void Awake()
    {
        if (instance == null)
        {
            instance = this;
            DontDestroyOnLoad(gameObject);
            RegisterServices();
        }
        else if (instance != this)
        {
            Destroy(gameObject);
        }
    }
    
    void RegisterServices()
    {
        // 注册存储服务
        if (useFileStorage)
        {
            Register<IDataStorage>(new FileDataStorage());
        }
        else
        {
            Register<IDataStorage>(new PlayerPrefsDataStorage());
        }
        
        // 注册日志服务
        if (useUnityLogger)
        {
            Register<ILogger>(new UnityLogger());
        }
        
        // 注册事件系统
        Register<IEventSystem>(new SimpleEventSystem());
    }
    
    public void Register<T>(T service)
    {
        services[typeof(T)] = service;
    }
    
    public T Get<T>()
    {
        Type serviceType = typeof(T);
        if (services.ContainsKey(serviceType))
        {
            return (T)services[serviceType];
        }
        throw new InvalidOperationException($"服务 {serviceType.Name} 未注册");
    }
    
    [Button("显示所有已注册的服务")]
    public void ShowRegisteredServices()
    {
        Debug.Log("已注册的服务:");
        foreach (var service in services)
        {
            Debug.Log($"- {service.Key.Name}: {service.Value.GetType().Name}");
        }
    }
}
```

#### 4. 遵循 DIP 的高层模块

```csharp
// ✅ 遵循依赖倒置原则的 GameManager
using Sirenix.OdinInspector;
using UnityEngine;
using System;

public class GameManager : MonoBehaviour
{
    [Title("依赖服务")]
    [ShowInInspector, ReadOnly] private IDataStorage dataStorage;
    [ShowInInspector, ReadOnly] private ILogger logger;
    [ShowInInspector, ReadOnly] private IEventSystem eventSystem;
    
    [Title("游戏状态")]
    [SerializeField] private int playerLevel = 1;
    [SerializeField] private float playerHealth = 100f;
    [SerializeField] private int playerScore = 0;
    
    void Start()
    {
        // 依赖注入：通过抽象接口获取服务
        InjectDependencies();
        
        // 设置事件监听
        SetupEventListeners();
        
        logger.Log("GameManager 已初始化");
    }
    
    void InjectDependencies()
    {
        try
        {
            dataStorage = ServiceContainer.Instance.Get<IDataStorage>();
            logger = ServiceContainer.Instance.Get<ILogger>();
            eventSystem = ServiceContainer.Instance.Get<IEventSystem>();
        }
        catch (Exception ex)
        {
            Debug.LogError($"依赖注入失败: {ex.Message}");
        }
    }
    
    void SetupEventListeners()
    {
        eventSystem.Subscribe<PlayerLevelUpEvent>(OnPlayerLevelUp);
        eventSystem.Subscribe<PlayerHealthChangedEvent>(OnPlayerHealthChanged);
    }
    
    [Button("保存游戏")]
    public void SaveGame()
    {
        try
        {
            var gameData = new GameData
            {
                level = playerLevel,
                health = playerHealth,
                score = playerScore,
                saveTime = DateTime.Now.ToString()
            };
            
            string jsonData = JsonUtility.ToJson(gameData, true);
            dataStorage.SaveData(jsonData, "savegame");
            
            logger.Log("游戏已保存");
        }
        catch (Exception ex)
        {
            logger.LogError($"保存游戏失败: {ex.Message}");
        }
    }
    
    [Button("加载游戏")]
    public void LoadGame()
    {
        try
        {
            if (dataStorage.HasData("savegame"))
            {
                string jsonData = dataStorage.LoadData("savegame");
                var gameData = JsonUtility.FromJson<GameData>(jsonData);
                
                playerLevel = gameData.level;
                playerHealth = gameData.health;
                playerScore = gameData.score;
                
                logger.Log($"游戏已加载（保存时间: {gameData.saveTime}）");
            }
            else
            {
                logger.LogWarning("未找到存档文件");
            }
        }
        catch (Exception ex)
        {
            logger.LogError($"加载游戏失败: {ex.Message}");
        }
    }
    
    [Button("玩家升级")]
    public void LevelUp()
    {
        playerLevel++;
        eventSystem.Publish(new PlayerLevelUpEvent { NewLevel = playerLevel });
    }
    
    void OnPlayerLevelUp(PlayerLevelUpEvent eventData)
    {
        logger.Log($"玩家升级到 {eventData.NewLevel} 级！");
    }
    
    void OnPlayerHealthChanged(PlayerHealthChangedEvent eventData)
    {
        logger.Log($"玩家血量变为 {eventData.NewHealth}");
    }
    
    void OnDestroy()
    {
        if (eventSystem != null)
        {
            eventSystem.Unsubscribe<PlayerLevelUpEvent>(OnPlayerLevelUp);
            eventSystem.Unsubscribe<PlayerHealthChangedEvent>(OnPlayerHealthChanged);
        }
    }
}

// ✅ 事件数据类
[System.Serializable]
public class GameData
{
    public int level;
    public float health;
    public int score;
    public string saveTime;
}

public class PlayerLevelUpEvent
{
    public int NewLevel { get; set; }
}

public class PlayerHealthChangedEvent
{
    public float NewHealth { get; set; }
}
```

### DIP 的优势

1. **降低耦合度**：高层模块不直接依赖低层实现
2. **提高灵活性**：可以轻松替换不同的实现
3. **便于测试**：可以轻松模拟依赖的服务
4. **易于扔展**：添加新的实现无需修改高层代码

!!! tip "Odin Inspector 的 DIP 支持"
    - 使用 `[ShowInInspector]` 显示注入的依赖服务
        - 使用 `[Button]` 提供服务测试功能
        - 使用 `[ReadOnly]` 防止意外修改注入的服务
        - 使用 `[Title]` 组织不同类型的依赖关系

## Unity 实战应用案例：角色系统重构

让我们通过一个完整的案例来看看如何将 SOLID 原则应用到 Unity 项目中。我们将重构一个传统的角色系统，使其遵循所有 SOLID 原则。

### 原始设计：违反 SOLID 原则

这是一个典型的“神类”设计：

```csharp
// ❌ 违反多个 SOLID 原则的角色系统
public class BadCharacter : MonoBehaviour
{
    // 违反 SRP：一个类承担太多职责
    public float health = 100f;
    public float maxHealth = 100f;
    public float moveSpeed = 5f;
    public float attackDamage = 25f;
    public float attackRange = 2f;
    public string[] inventory = new string[10];
    public int level = 1;
    public int experience = 0;
    
    // 各种系统混在一起
    void Update()
    {
        HandleMovement();
        HandleCombat();
        HandleLevelUp();
        UpdateUI();
        HandleInventory();
    }
    
    // 违反 OCP：添加新角色类型需要修改这个方法
    void HandleMovement()
    {
        // 处理不同类型的移动
        if (gameObject.name.Contains("Warrior"))
        {
            // 战士移动逻辑
        }
        else if (gameObject.name.Contains("Mage"))
        {
            // 法师移动逻辑
        }
        // ... 更多类型
    }
    
    // 其他方法...
}
```

### 重构后：遵循 SOLID 原则的设计

让我们将这个系统重构为遵循 SOLID 原则的架构。

#### 1. 应用 SRP：职责分离

```csharp
// ✅ 健康系统（单一职责）
using Sirenix.OdinInspector;
using UnityEngine;
using UnityEngine.Events;

public class HealthSystem : MonoBehaviour
{
    [Title("健康配置")]
    [SerializeField, Range(1, 1000)] private float maxHealth = 100f;
    [SerializeField] private bool regenerateHealth = true;
    [SerializeField, ShowIf("regenerateHealth")] private float regenRate = 5f;
    
    [ShowInInspector, ReadOnly, ProgressBar(0, "maxHealth", ColorGetter = "GetHealthColor")]
    private float currentHealth;
    
    [Title("事件")]
    public UnityEvent<float> OnHealthChanged;
    public UnityEvent<float, float> OnDamageTaken; // damage, remaining health
    public UnityEvent OnDeath;
    public UnityEvent OnFullyHealed;
    
    public float CurrentHealth => currentHealth;
    public float MaxHealth => maxHealth;
    public float HealthPercentage => currentHealth / maxHealth;
    public bool IsAlive => currentHealth > 0;
    public bool IsAtFullHealth => Mathf.Approximately(currentHealth, maxHealth);
    
    void Start()
    {
        currentHealth = maxHealth;
    }
    
    void Update()
    {
        if (regenerateHealth && !IsAtFullHealth && IsAlive)
        {
            Heal(regenRate * Time.deltaTime);
        }
    }
    
    public void TakeDamage(float damage)
    {
        if (!IsAlive || damage <= 0) return;
        
        float previousHealth = currentHealth;
        currentHealth = Mathf.Max(0, currentHealth - damage);
        
        OnHealthChanged?.Invoke(currentHealth);
        OnDamageTaken?.Invoke(damage, currentHealth);
        
        if (!IsAlive)
        {
            OnDeath?.Invoke();
        }
    }
    
    public void Heal(float amount)
    {
        if (!IsAlive || amount <= 0 || IsAtFullHealth) return;
        
        currentHealth = Mathf.Min(maxHealth, currentHealth + amount);
        OnHealthChanged?.Invoke(currentHealth);
        
        if (IsAtFullHealth)
        {
            OnFullyHealed?.Invoke();
        }
    }
    
    public void SetMaxHealth(float newMaxHealth)
    {
        float ratio = HealthPercentage;
        maxHealth = Mathf.Max(1, newMaxHealth);
        currentHealth = maxHealth * ratio;
        OnHealthChanged?.Invoke(currentHealth);
    }
    
    private Color GetHealthColor()
    {
        float percentage = HealthPercentage;
        if (percentage > 0.6f) return Color.green;
        if (percentage > 0.3f) return Color.yellow;
        return Color.red;
    }
}

// ✅ 经验系统（单一职责）
public class ExperienceSystem : MonoBehaviour
{
    [Title("经验配置")]
    [SerializeField] private int currentLevel = 1;
    [SerializeField] private int currentExperience = 0;
    [SerializeField] private int baseExperienceToNext = 100;
    [SerializeField, Range(1.1f, 2f)] private float experienceMultiplier = 1.5f;
    
    [ShowInInspector, ReadOnly]
    private int experienceToNextLevel;
    
    [Title("事件")]
    public UnityEvent<int> OnLevelUp;
    public UnityEvent<int> OnExperienceGained;
    
    public int Level => currentLevel;
    public int Experience => currentExperience;
    public int ExperienceToNext => experienceToNextLevel;
    public float ExperienceProgress => (float)currentExperience / experienceToNextLevel;
    
    void Start()
    {
        CalculateExperienceToNext();
    }
    
    public void GainExperience(int amount)
    {
        if (amount <= 0) return;
        
        currentExperience += amount;
        OnExperienceGained?.Invoke(amount);
        
        while (currentExperience >= experienceToNextLevel)
        {
            LevelUp();
        }
    }
    
    void LevelUp()
    {
        currentExperience -= experienceToNextLevel;
        currentLevel++;
        CalculateExperienceToNext();
        OnLevelUp?.Invoke(currentLevel);
    }
    
    void CalculateExperienceToNext()
    {
        experienceToNextLevel = Mathf.RoundToInt(baseExperienceToNext * Mathf.Pow(experienceMultiplier, currentLevel - 1));
    }
}
```

#### 2. 应用 OCP：角色类型的可扩展设计

```csharp
// ✅ 角色能力接口（对扩展开放）
public interface IMoveable
{
    float MoveSpeed { get; set; }
    void Move(Vector3 direction);
    void Stop();
}

public interface IAttacker
{
    float AttackDamage { get; }
    float AttackRange { get; }
    void Attack(GameObject target);
    bool CanAttack(GameObject target);
}

public interface ICaster
{
    float ManaPoints { get; }
    float MaxMana { get; }
    void CastSpell(string spellName, Vector3 target);
    bool HasEnoughMana(float cost);
}

// ✅ 基础角色类定义抽象行为
using Sirenix.OdinInspector;
using UnityEngine;

public abstract class Character : MonoBehaviour
{
    [Title("角色基础信息")]
    [SerializeField] protected string characterName;
    [SerializeField] protected int characterLevel = 1;
    
    [Title("组件引用")]
    [SerializeField, Required] protected HealthSystem healthSystem;
    [SerializeField, Required] protected ExperienceSystem experienceSystem;
    
    [ShowInInspector, ReadOnly]
    public string CharacterName => characterName;
    
    [ShowInInspector, ReadOnly]
    public int Level => experienceSystem ? experienceSystem.Level : characterLevel;
    
    protected virtual void Start()
    {
        InitializeCharacter();
        SetupEventListeners();
    }
    
    protected virtual void InitializeCharacter()
    {
        if (string.IsNullOrEmpty(characterName))
            characterName = GetType().Name;
    }
    
    protected virtual void SetupEventListeners()
    {
        if (healthSystem != null)
        {
            healthSystem.OnDeath.AddListener(OnCharacterDeath);
        }
        
        if (experienceSystem != null)
        {
            experienceSystem.OnLevelUp.AddListener(OnCharacterLevelUp);
        }
    }
    
    protected virtual void OnCharacterDeath()
    {
        Debug.Log($"{characterName} 已死亡");
    }
    
    protected virtual void OnCharacterLevelUp(int newLevel)
    {
        Debug.Log($"{characterName} 升级到 {newLevel} 级！");
    }
    
    protected virtual void OnDestroy()
    {
        if (healthSystem != null)
        {
            healthSystem.OnDeath.RemoveListener(OnCharacterDeath);
        }
        
        if (experienceSystem != null)
        {
            experienceSystem.OnLevelUp.RemoveListener(OnCharacterLevelUp);
        }
    }
}

// ✅ 战士类：扩展而非修改
public class Warrior : Character, IMoveable, IAttacker
{
    [Title("战士属性")]
    [SerializeField, Range(1, 20)] private float moveSpeed = 5f;
    [SerializeField, Range(10, 100)] private float attackDamage = 30f;
    [SerializeField, Range(1, 5)] private float attackRange = 2.5f;
    [SerializeField, Range(0.5f, 3f)] private float attackCooldown = 1.2f;
    
    private float lastAttackTime;
    
    public float MoveSpeed 
    { 
        get => moveSpeed; 
        set => moveSpeed = Mathf.Max(0, value); 
    }
    
    public float AttackDamage => attackDamage;
    public float AttackRange => attackRange;
    
    public void Move(Vector3 direction)
    {
        if (healthSystem && healthSystem.IsAlive)
        {
            transform.Translate(direction.normalized * moveSpeed * Time.deltaTime);
        }
    }
    
    public void Stop()
    {
        // 停止移动逻辑
    }
    
    public bool CanAttack(GameObject target)
    {
        if (!healthSystem || !healthSystem.IsAlive) return false;
        if (Time.time < lastAttackTime + attackCooldown) return false;
        
        float distance = Vector3.Distance(transform.position, target.transform.position);
        return distance <= attackRange;
    }
    
    public void Attack(GameObject target)
    {
        if (!CanAttack(target)) return;
        
        var targetHealth = target.GetComponent<HealthSystem>();
        if (targetHealth != null)
        {
            targetHealth.TakeDamage(attackDamage);
            lastAttackTime = Time.time;
            Debug.Log($"{characterName} 攻击了 {target.name}，造成 {attackDamage} 点伤害");
        }
    }
}

// ✅ 法师类：新的角色类型，无需修改现有代码
public class Mage : Character, IMoveable, ICaster
{
    [Title("法师属性")]
    [SerializeField, Range(1, 15)] private float moveSpeed = 3f;
    [SerializeField, Range(20, 200)] private float maxMana = 100f;
    [SerializeField, Range(1, 10)] private float manaRegenRate = 5f;
    
    [ShowInInspector, ReadOnly, ProgressBar(0, "maxMana", r: 0.2f, g: 0.5f, b: 1f)]
    private float currentMana;
    
    public float MoveSpeed 
    { 
        get => moveSpeed; 
        set => moveSpeed = Mathf.Max(0, value); 
    }
    
    public float ManaPoints => currentMana;
    public float MaxMana => maxMana;
    
    protected override void Start()
    {
        base.Start();
        currentMana = maxMana;
    }
    
    void Update()
    {
        RegenerateMana();
    }
    
    public void Move(Vector3 direction)
    {
        if (healthSystem && healthSystem.IsAlive)
        {
            transform.Translate(direction.normalized * moveSpeed * Time.deltaTime);
        }
    }
    
    public bool HasEnoughMana(float cost)
    {
        return currentMana >= cost;
    }
    
    public void CastSpell(string spellName, Vector3 target)
    {
        float manaCost = GetSpellCost(spellName);
        if (!HasEnoughMana(manaCost)) return;
        
        currentMana -= manaCost;
        
        switch (spellName.ToLower())
        {
            case "fireball":
                CastFireball(target);
                break;
            case "heal":
                CastHeal();
                break;
            default:
                Debug.LogWarning($"未知法术: {spellName}");
                break;
        }
    }
    
    void RegenerateMana()
    {
        if (currentMana < maxMana)
        {
            currentMana = Mathf.Min(maxMana, currentMana + manaRegenRate * Time.deltaTime);
        }
    }
    
    float GetSpellCost(string spellName)
    {
        return spellName.ToLower() switch
        {
            "fireball" => 20f,
            "heal" => 30f,
            _ => 10f
        };
    }
    
    void CastFireball(Vector3 target)
    {
        Debug.Log($"{characterName} 释放了火球术！");
        // 火球法术逻辑
    }
    
    void CastHeal()
    {
        if (healthSystem != null)
        {
            healthSystem.Heal(50f);
            Debug.Log($"{characterName} 使用了治疗法术！");
        }
    }
}
```

#### 3. 应用 LSP 和 ISP：正确的继承和接口设计

```csharp
// ✅ 遵循 LSP：所有子类都可以替换基类
// 遵循 ISP：接口精简且不强制依赖

public class CharacterManager : MonoBehaviour
{
    [Title("角色管理")]
    [SerializeField] private Character[] allCharacters;
    
    [ShowInInspector, ReadOnly]
    private List<IMoveable> moveableCharacters = new List<IMoveable>();
    
    [ShowInInspector, ReadOnly]
    private List<IAttacker> attackerCharacters = new List<IAttacker>();
    
    [ShowInInspector, ReadOnly]
    private List<ICaster> casterCharacters = new List<ICaster>();
    
    void Start()
    {
        CategorizeCharacters();
    }
    
    void CategorizeCharacters()
    {
        moveableCharacters.Clear();
        attackerCharacters.Clear();
        casterCharacters.Clear();
        
        foreach (var character in allCharacters)
        {
            if (character is IMoveable moveable)
                moveableCharacters.Add(moveable);
                
            if (character is IAttacker attacker)
                attackerCharacters.Add(attacker);
                
            if (character is ICaster caster)
                casterCharacters.Add(caster);
        }
    }
    
    [Button("所有角色向前移动")]
    public void MoveAllCharactersForward()
    {
        foreach (var moveable in moveableCharacters)
        {
            moveable.Move(Vector3.forward);
        }
    }
    
    [Button("所有攻击型角色攻击目标")]
    public void AllAttackersAttackTarget()
    {
        GameObject target = GameObject.FindWithTag("Enemy");
        if (target != null)
        {
            foreach (var attacker in attackerCharacters)
            {
                attacker.Attack(target);
            }
        }
    }
}
```

## 总结

SOLID 原则是建设高质量 Unity 项目的基石。通过本文的学习，你应该掌握了：

- **SOLID 五大原则的核心概念**和实用方法
- **在 Unity 中的具体应用**，从简单的组件分离到复杂的架构设计
- **Odin Inspector 的强大支持**，提升开发效率和代码质量
- **实战经验和最佳实践**，避免常见陷阱

### 下一步学习建议

1. **实践练习**：找一个小项目尝试应用所学原则
2. **深入学习**：研究设计模式如何体现 SOLID 原则
3. **工具掌握**：学习更多 Odin Inspector 高级特性
4. **团队分享**：将所学知识分享给团队成员

### 参考资源

- [Unity 官方文档](https://docs.unity3d.com/)
- [Odin Inspector 文档](https://odininspector.com/)
- [Unity 官方 Clean Code 电子书：C# 代码风格指南](https://unity.com/resources/create-code-c-sharp-style-guide-e-book)
- [Unity 官方设计模式和 SOLID 原则电子书](https://unity.com/resources/design-patterns-solid-ebook)
- [Martin Fowler 的设计文章](https://martinfowler.com/)

---

希望这篇文章能帮助你在 Unity 开发中更好地应用 SOLID 原则！如果你有任何问题或建议，欢迎在评论区交流讨论。