# zola-deploy-pages

Github action to deploy zola directly to Github Pages without an extra branch
with the builtin math branch of cestef. I had some articles that needed this
functionality and I don't want to wait anymore. The zola build is from
[cestef/zola](https://github.com/cestef/zola/tree/feature/math-rendering)
at commit `8f335f1`.

This approach also doesn't require creating an extra branch for the deployment
so make sure to use set Pages to `Source: Github Actions`. The downside is that
it doesn't allow for a private site source since the site repo needs to be
public. `actions/upload-pages-artifact@v3` and `actions/deploy-pages@v4` both
operate in the scope of the current repository, and the former in particular
works on some echo magic to work.

If you want privacy you're best choice is probably
[shalzz/zola-deploy-action](https://github.com/shalzz/zola-deploy-action/tree/master)

## TODO
- [ ] Enable SVG minification
- [ ] Store SVG cache between site builds
