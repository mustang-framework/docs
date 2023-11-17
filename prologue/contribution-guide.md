# Contribution Guide

### Bug Reports[​](https://apiato.io/docs/prologue/contribution-guide#bug-reports) <a href="#bug-reports" id="bug-reports"></a>

To encourage active collaboration, Mustang strongly encourages pull requests, not just bug reports. Pull requests will only be reviewed when marked as "ready for review" (not in the "draft" state), and all tests for new features are passing. Lingering, non-active pull requests left in the "draft" state will be closed after a few days.

However, if you file a bug report, your issue should contain a title and a clear description of the issue. You should also include as much relevant information as possible and a code sample that demonstrates the issue. The goal of a bug report is to make it easy for yourself - and others - to replicate the bug and develop a fix.

Remember, bug reports are created in the hope that others with the same problem will be able to collaborate with you on solving it. Do not expect that the bug report will automatically see any activity or that others will jump to fix it. Creating a bug report serves to help you and others start on the path of fixing the problem. If you want to chip in, you can help out by fixing [any bugs listed in our issue trackers](https://github.com/mustang-framework/mustang/issues). You must be authenticated with GitHub to view all of Musnag's issues.

If you notice improper DocBlock, Psalm, or IDE warnings while using ApiatMustango, do not create a GitHub issue. Instead, please submit a pull request to fix the problem.

### Core Development Discussion[​](https://apiato.io/docs/prologue/contribution-guide#core-development-discussion) <a href="#core-development-discussion" id="core-development-discussion"></a>

You may propose new features or improvements to existing Mustang behavior in the Mustang framework repository's GitHub discussion board. If you propose a new feature, please be willing to implement at least some of the code that would be needed to complete the feature.

### Which Branch?[​](https://apiato.io/docs/prologue/contribution-guide#which-branch) <a href="#which-branch" id="which-branch"></a>

**All** bug fixes should be sent to the latest version that supports bug fixes. Bug fixes should **never** be sent to the `master` branch unless they fix features that exist only in the upcoming release.

**Minor** features that are **fully backward compatible** with the current release may be sent to the latest stable branch.

**Major** new features or features with breaking changes should always be sent to the `master` branch, which contains the upcoming release.

### Security Vulnerabilities[​](https://apiato.io/docs/prologue/contribution-guide#security-vulnerabilities) <a href="#security-vulnerabilities" id="security-vulnerabilities"></a>

If you discover a security vulnerability within Mustang, please email Mohammad Alavi at tech@kingcode.ovh. All security vulnerabilities will be promptly addressed.

### Coding Style[​](https://apiato.io/docs/prologue/contribution-guide#coding-style) <a href="#coding-style" id="coding-style"></a>

Mustang follows the [PSR-12](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-12-extended-coding-style-guide.md) coding standard with some modifications and the [PSR-4](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md) autoloading standard.

#### PHPDoc[​](https://apiato.io/docs/prologue/contribution-guide#phpdoc) <a href="#phpdoc" id="phpdoc"></a>

Below is an example of a valid Mustang documentation block. Note that the `@param` attribute is followed by one space, the argument type, one more space, and finally the variable name:

```
    /**
     * Register a binding with the container.
     *
     * @param string|array $abstract
     * @param \Closure|string|null $concrete
     * @param bool $shared
     * @return void
     *
     * @throws \Exception
     */
    public function bind($abstract, $concrete = null, $shared = false)
    {
        // ...
    }
```

When the `@param` or `@return` attributes are redundant due to the use of native types, they can be removed:

```
    /**
     * Execute the job.
     */
    public function handle(AudioProcessor $processor): void
    {
        //
    }
```

However, when the native type is generic, please specify the generic type through the use of the `@param` or `@return` attributes:

```
    /**
     * Get the attachments for the message.
     *
     * @return array<int, \Illuminate\Mail\Mailables\Attachment>
     */
    public function attachments(): array
    {
        return [
            Attachment::fromStorage('/path/to/file'),
        ];
    }
```

#### GitHub Workflow[​](https://apiato.io/docs/prologue/contribution-guide#github-workflow) <a href="#github-workflow" id="github-workflow"></a>

Don't worry if your code styling isn't perfect! GitHub Actions will automatically merge any style fixes into the Mustang repository after pull requests are merged. This allows us to focus on the content of the contribution and not the code style.

### Code of Conduct[​](https://apiato.io/docs/prologue/contribution-guide#code-of-conduct) <a href="#code-of-conduct" id="code-of-conduct"></a>

The Mustang code of conduct is derived from the Ruby code of conduct. Any violations of the code of conduct may be reported to Mohammad Alavi (dev@kingcode.ovh):

* Participants will be tolerant of opposing views.
* Participants must ensure that their language and actions are free of personal attacks and disparaging personal remarks.
* When interpreting the words and actions of others, participants should always assume good intentions.
* Behavior that can be reasonably considered harassment will not be tolerated.
