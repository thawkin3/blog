# ESLint Warnings Are an Anti-Pattern

ESLint offers three settings for any given rule: `error`, `warn`, and `off`. The `error` setting will make ESLint fail if it encounters any violations of the rule, the `warn` setting will make ESLint report issues found but will not fail if it encounters any violations of the rule, and the `off` setting disables the rule.

I'd like to argue that using `warn` is an anti-pattern. Why? Because either you care about something, or you don't. The rule is either important to follow and violations should be fixed, or the rule is not important and developers shouldn't worry about it. Therefore, use `error` or `off`.

---

## The Problem with Warnings

Now, there's no real issue with using the `warn` setting. The problem though is that when ESLint rule violations aren't enforced, developers tend to ignore them. This means that warnings will pile up in the ESLint output, creating a lot of noise and clutter.

So do you care about the rules that are being violated or not? If not, why do you have the rule enabled? If a rule serves no purpose and developers aren't addressing the warnings, get rid of the rule. If the rule is important, set it to `error` so that developers don't have the option of ignoring it.

---

## One Caveat

There is one use case for the `warn` setting that I feel is valid, so let's address that. Remember, only a Sith deals in absolutes.

When introducing a new ESLint rule to your codebase, you may find that you have too many violations to fix all at once. In this situation, what are you to do? You want to keep your ESLint script passing, especially if you enforce it in your CI pipeline (which you should!). It may be counterproductive to add an `eslint-disable-next-line` comment along with a `TODO` comment for every violation of this rule as well.

So, if you're adding a new ESLint rule and find that for whatever reason you can't clean up the violations all at once, set the new rule to `warn`, at least for now. Understand though that this is a temporary setting and that it should be your goal to clean up the warnings as quickly as possible. Then, once the rule violations have been taken care of, you can change the rule setting to `error`.

---

## Conclusion

ESLint warnings are an anti-pattern. Use only the `error` or `off` settings, and reserve using the `warn` setting only as a temporary stopgap.
