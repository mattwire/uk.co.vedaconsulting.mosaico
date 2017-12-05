# APIv3

CiviCRM's APIv3 framework provides a way for consumers to manage data and call services.  APIv3 can be called in many ways
(such as PHP, REST, and CLI). For a general introduction, see [APIv3 Intro](https://docs.civicrm.org/dev/en/latest/api/).

This extension defines a few new APIs:

* `Job.mosaico_migrate`: If you need to perform an automated migration from v1.x to v2.x, use this API to copy all
  v1.x templates to v2.x.
* `Job.mosaico_purge`: If you need to perform an automated migration from v1.x to v2.x, use this API to clear out the
  old v1.x templates.
* `MosaicoTemplate.*`: This API provides access to the user-configurable templates.  It supports all standard CRUD
  actions (`get`, `create`, `delete`etc). Its data-structure closely adheres to Mosaico's canonical storage format.
* `MosaicoBaseTemplate.get`: This API provides access to the *base templates*. A base template (such as `versafix-1`)
  defines the HTML blocks that are available for drag/drop in the Mosaico palette. Note: This API is *read-only*.
  To define custom templates, see the section on "Base templates".

# Base templates

A base template defines the HTML blocks that are available for drag/drop in the Mosaico palette. The standard distribution
of Mosaico includes a few base templates, such as `versafix-1`.

The upstream Mosaico project provides a tutorial on how to develop a custom base template:

https://github.com/voidlabs/mosaico/blob/master/templates/tutorial/mosaico-tutorial.md

The new template is essentially a folder with a couple standard files.  Once you've developed these files, you'll need
a way to *deploy* the folder, such as:

* __Drop-in folder__: On any site, create a folder dedicated to custom base templates.  By default, this is
  `[civicrm.files]/mosaico_tpl`.  (`[civicrm.files]` is a variable that resolves somewhere under the CMS's data
  folder.) For example, if you deployed a template `foobar` on a typical Drupal 7 site, the full path to the template HTML
  might be `/var/www/sites/default/files/civicrm/mosaico_tpl/foobar/template-foobar.html`.  (The folder name can be
  customized in "Administer => CiviMail => Mosaico Settings".)
* __hook_civicrm_mosaicoBaseTemplates__: CiviCRM's [hook system](https://docs.civicrm.org/dev/en/latest/hooks/) provides a
  way to programmatically register templates. For example, here an extension named `mymodule` registers a base-template
  named `foobar`:
  ```php
  <?php
  use CRM_Mymodule_ExtensionUtil as E;
  function mymodule_civicrm_mosaicoBaseTemplates(&$templates) {
    $templates['foobar'] = array(
      'name' => 'foobar',
      'title' => 'Foo Bar',
      'path' => E::url('foobar/template-foobar.html'),
      'thumbnail' => E::url('foobar/edres/_full.png'),
    );
  }
  ```

# Delivery

After designing a mailing, email messages are composed and delivered through FlexMailer.  To programmaticaly tap into the
composition and delivery process, see the [FlexMailer developer docs](https://docs.civicrm.org/flexmailer/en/latest/).