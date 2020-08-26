# ReadMe Synchronization Orb for CircleCI

This is a prototype CircleCI orb that wraps a subset [ReadMe CLI](https://github.com/readmeio/rdme), allowing you to synchronize documents from your CircleCI pipeline.

## Available Jobs

* `readme-sync/sync-docs`
* `readme-sync/sync-spec`

## Configuration

### Project Token

All jobs require a ReadMe API key. This is passed into each sync job, preferably as an environment variable:

```
workflows:
  version: 2
    deploy_docs:
      jobs:
        - sync-docs:
            readme-api-key: README_API_KEY
```

## Adding a new document

### Get the ReadMe category ID

If you are adding a new document to a new category on ReadMe, you will need to retrieve the new category ID. The simplest way to 
do this would be to use their [Category API](https://docs.readme.com/developers/reference/categories). If the category already exists
and there are existing files for that category, simply copy the category ID from the metadata header, as described [below](#add-the-proper-document-metadata).

### Add the proper document metadata

ReadMe's Markdown flavor uses a metadata header at the top of each document that contains info such as the title, category ID,
parent document ID, and so forth. These fields map to the corresponding REST API parameters in the [destructive API endpoints](https://docs.readme.com/developers/reference/docs#updatedoc):


```
---
title: Your Document Title
category: some-category-id
parentDoc: your-parent-doc <-- optional
---
```

### Parent-child relationships

This orb currently uses the following folder structure to represent a parent-child relationship:

```
my-docs/
   parent-doc.md
   parent-doc/
      child-doc.md
```

This allows for the parent to be synced first, and then have its ID retrieved to be used to create the parent-child connection
with the child document subfolder.

## Configuring an API spec

A job is available to sync an API spec: `sync-spec`.

### Required parmeters

#### API ID

Syncing an API spec requires an API ID. This can either be passed in as a parameter to the job, or you can create
an `id` file that lives alongside the spec, containing only the ID of the API.

```
my-api/
   my-api.yaml
   id
```

 The ID for an API can be retrieved directly from the `API Reference` section of the ReadMe dashboard, or via the [category API](https://docs.readme.com/developers/reference/categories) (an API is actually a category under the hood).

### Resource docs

Similar to the [parent-child relationship](#parent-child-relationships) for docs, accompanying resource docs for an API can live alongside the API spec:

```
my-api/
   my-api.yaml
   my-resource.md # The root resource doc
   my-resource/
    get-resource.md # A doc for the resource method
    add-resource.md
   another-resource.md
   another-resource/
    update-resource.md
    delete-resource.md
```

Note that these docs should be named to match the correct slug of the API resource in ReadMe. They should also contain the proper
metadata header as described [here](#add-the-proper-document-metadata). The category ID for the API can be had via the [category API](https://docs.readme.com/developers/reference/categories).

## Deleting documents

This orb does not currently support this feature, since the ReadMe CLI does not also support it. However, docs can be deleted via either the ReadMe dashboard or their REST API.

## Contributing

Feel free to file an issue or make any pull requests! We will get back to you as soon as possible.