# Django Model Related Instructions

* When defining a model, always import and use `gettext_lazy` for verbose names:

    ```
    from django.utils.translation import gettext_lazy as _
    ```

* When defining a model, always add `class Meta:` and `__str__` method,
  `natural_key` method, `objects` property, `natural_key.dependencies` 
  (if it’s needed), model’s manager with `get_by_natural_key` method with
  following properties:

    ```
    class <Modelname>Manager(models.Manager): # (e.g PostManager)
        def get_by_natural_key(self, <field_lookup_param>): # (e.g slug)
            return self.get(<field_name>=<field_lookup_param>) # (e.g slug=slug)

    class <Modelname>(models.Model):
        # field definintions
        
        objects = <Modelname>Manager()
    
        def __str__(self):
            return f'{self.<field_name>}'
            
        def natural_key(self):
            return (self.<field_name>,)
        
        # if you need it for get_by_natural_key manager method
        natural_key.dependencies = ['<app>.<modelname>'] # (e.g core.company)
        
        class Meta:
            app_label = '<app_label>' # (e.g core)
            verbose_name = _('<Modelname>') # (e.g Post)
            verbose_name_plural = _('<Modelname_plural>') # (e.g Posts)
            db_table = '<database_table_name>' # (e.g post)
    ````

* When defining relation fields such as `models.ForeignKey` always 
  add `to=` as related model, `related_name=` and `related_query_name=` for 
  query lookups with following:
  
    ```
    (e.g company field for Post model)
    <field_name> = models.ForeignKey( # (e.g company)
       to='<RelatedModel>', # (e.g Company)
       related_name='<modelname_plural>', # (e.g posts)
       related_query_name='<modelname>', # (e.g post)
       on_delete=models.CASCADE,
       verbose_name=_('<FieldName>'), # (e.g Company)
    )
    ```

* When defining relation fields such as `models.ManyToManyField` always add
  `to=` as related model, `related_name=` and `related_query_name=`for 
  query lookups with following:

    ```
    (e.g recruitments field for Company model)
    <field_name_plural> = models.ManyToManyField( # (e.g recruitments)
        to='<RelatedModel>', # (e.g Candidate)
        related_name='<modelname_plural>', # (e.g companies)
        related_query_name='<modelname>', # (e.g company)
        blank=True,
        verbose_name=_('<FieldName>'), # (e.g Recruitments)
    )
    ```

* When you need to define `models.ManyToManyField`, tell user to consider
  about Many-To-Many with using `through` relation, tell about advantages of
  associative models/tables:

    ```
    (e.g recruitments field for Company model)
    <field_name_plural> = models.ManyToManyField(
        to='<RelatedModel>', # (e.g Candidate)
        through='<AssociativeModel>', # (e.g Recruitment)
        through_fields=('<associative_model_field1>', '<associative_model_field2>'), # (e.g 'company', 'candidate')
        related_name='<modelname_plural>', # (e.g companies)
        related_query_name='<modelname>', # (e.g company)
        blank=True,
        verbose_name=_('<FieldName>'), # (e.g Recruitments)
    )
    ```

* When defining a model, you should follow the official Django coding
  conventions as outlined in in https://docs.djangoproject.com/en/5.2/internals/contributing/writing-code/coding-style/
  
  - All database fields
  - Custom manager attributes
  - `class Meta`
  - `def __str__()` and other Python magic methods
  - `def save()`
  - `def get_absolute_url()`
  - Any custom methods

* Always localize texts, in templates, according to question, use:

  - `{% translate "string to translate" as VAR %}`
  - `{% blocktranslate trimmed %}string to translate{% endblocktranslate %}`
  - `{% blocktranslate trimmed with var1 as var2 %}string to translate {{ var2 }}{% endblocktranslate %}`
  - `{{ _('string to translate') }}`

* When defining forms or model-forms, always add localized labels. (e.g password = forms.CharField(label=_('Password'), ...) ) 
