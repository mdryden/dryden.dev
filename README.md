[![Deploy](https://github.com/mdryden/dryden.dev/actions/workflows/deploy.yml/badge.svg)](https://github.com/mdryden/dryden.dev/actions/workflows/deploy.yml)

# dryden.dev

The source which drives my personal profile site at [dryden.dev](https://dryden.dev), and perhaps a good example of how you can generate and deploy a static site built with Hugo using GitHub Actions and Google Cloud Storage.

## Configuration

If you were to clone this repository, or to borrow the github actions and adjust to work with your own site, here's what you'd need to do:

1. Configure your DNS with a CNAME that points to "c.storage.googleapis.com."

I use CloudFlare to host my DNS, because it's easy, free, and deals with HTTPS for me.

2. Create a bucket in Google Cloud with a name that matches the domain you are using. For example, my bucket name is "dryden.dev".

Note that if you didn't register your domain with Google Domains, you'll need to [jump through some hoops](https://cloud.google.com/storage/docs/domain-name-verification#verification) to prove you own the domain.

Make sure your bucket is public, and that the "allUsers" principle has the "Cloud Storage > Storage Object Viewer" role.

3. Navigate to the root of Cloud Storage, click the overflow menu (3 dots) beside your bucket, and go into "Edit Website Configuration". Set the index to "index.html" and the 404 to "404.html".

4. Create a GCP service account using the "IAM & Admin" panel, and grant it the "Cloud Storage > Storage Admin" and "Cloud Storage > Storage Object Admin" roles.

5. Create a JSON key for your service account.

6. Copy the contents of the JSON key file into a GitHub actions secret.

I called mine GCP_SERVICE_ACCOUNT_KEY. You can call your secret whatever you want, but you'll need to update [deploy.yml](.github/workflows/deploy.yml) to match.

7. Update deploy.yml with the correct bucket name.

In theory, that's it. If your DNS is resolving to your bucket, every commit to main which includes changes in the www folder should trigger a redeploy to GCP.
