# Process for creating, publishing and submitting for verification

This is a rough process of how to create, publish and submit a npm package for verification. Some details here may be different for you, the commands are just examples.

1. Create a GitHub repo (don't add a README, .gitignore or choose a license). e.g. https://github.com/goudbes/d2scp-test-components
2. Create a project locally with `create-react-library`
    ```
    npx create-react-library --template default --manager npm --repo https://github.com/goudbes/d2scp-test-components goudbes-d2scp-test-components 
    ```
3. Follow the instructions at github for "push an existing repository from the command line".
4. Run local verification
    ```
    npx -p "https://github.com/haheskja/scp-cli#master" dhis2-scp-cli -vvv verify
    ```
5. Tag to the version in package.json
    ```
    git tag v1.0.0
    ```
5. Update the version in package.json
    ```
    "version": "1.0.1"
    ```
6. Commit package.json with the new version
7. Push the created tag
    ```
    git push --tags
    ```
8. Login to npm with `npm login`
9. Checkout the tag `git checkout v1.0.0`
10. Publish the tag to npm `npm publish`


Verification:

1. Go to https://github.com/dhis2designlab/scp-whitelist/blob/main/list.csv
2. Create a pull request for adding the package (`goudbes-d2scp-test-components,1.0.0`)