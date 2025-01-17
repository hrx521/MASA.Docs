## 验证 - FluentValidation

提供基于[`FluentValidation`](https://www.nuget.org/packages/FluentValidation)的参数验证扩展

扩展验证支持了`zh-CN`(中文简体)、`en-US` (英语(美国)、`en-GB` (英语(英国)) 的本地化验证支持

## 使用

安装`Masa.Utils.Extensions.Validations.FluentValidation`

``` powershell
dotnet add package Masa.Utils.Extensions.Validations.FluentValidation
```

如何使用

```csharp
public class RegisterUser
{
    public string Account { get; set; }

    public string Password { get; set; }
}

public class RegisterUserValidator : AbstractValidator<RegisterUser>
{
    public RegisterUserValidator()
    {
        RuleFor(user => user.Account).Letter()
    }
}
```

## 源码解读

### 验证扩展支持

* Chinese: 中文验证
* Number: 数字验证
* Letter: 字母验证 (大小写均可)
* LowerLetter: 小写字母验证
* UpperLetter: 大写字母验证
* LetterNumber: 字母数字验证
* ChineseLetterUnderline: 中文字母下划线验证
* ChineseLetter： 中文字母验证
* ChineseLetterNumberUnderline: 中文字母数字下划线验证
* ChineseLetterNumber: 中文字母数字验证
* Phone: 手机号验证 (支持`zh-CN`、`en-US`、`en-GB`)
* IdCard: 身份验证 (支持`zh-CN`)
  * 目前身份验证仅支持中国15、18位身份证

> 不同国家的手机号码有着不同的规则, 并不是简单通过输入内容是否数字以及数字的长度来验证手机号码

<div class="custom-table">

|  国家  | 文化 | 验证规则  |
|  ----  | ----  | ----  |
| 中国 | `zh-CN` | `^(\+?0?86\-?)?1[345789]\d{9}$` |
| 美国 | `en-US` | `^(\+?1)?[2-9]\d{2}[2-9](?!11)\d{6}$` |
| 英国 | `en-GB` | `^(\+?44\|0)7\d{9}$` |

</div>

* IdCard: 身份证验证 (支持`zh-CN`)
* Url: Url地址验证
* Port: 端口验证
* Required: 必填验证 (与`NotEmpty`效果一致)

### 修改默认语言

GlobalValidationOptions.SetDefaultCulture("zh-CN");

针对`Phone`、`IdCard`等支持多语言的验证器, 其`culture`获取顺序为

指定`culture` -> (全局设置默认Culture) -> `CultureInfo.CurrentUICulture.Name`

### 本地化支持

更换默认验证器语言中的提示信息

```csharp
ValidatorOptions.Global.LanguageManager = new MasaLanguageManager();
```

> 如果未指定默认语言信息, 当使用`Masa提供的扩展验证, 并且未重新指定 LanguageManager, 将会导致获取到不正确的验证信息`