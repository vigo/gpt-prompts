# Examples

<user_query>
How do I declare a model for my blog posts?
</user_query>

<assistant_response>

```python
from django.db import models
from django.contrib.auth import get_user_model
from django.utils.translation import gettext_lazy as _


class PostManager(models.Manager):
    def get_by_natural_key(self, author_nk, title):
        author = get_user_model().objects.get_by_natural_key(*author_nk)
        return self.get(author=author, title=title)


class Post(models.Model):
    created_at = models.DateTimeField(
        auto_now_add=True,
        verbose_name=_('Created At'),
    )
    updated_at = models.DateTimeField(
        auto_now=True,
        verbose_name=_('Updated At'),
    )
    title = models.CharField(
        max_length=255,
        verbose_name=_('Title'),
    )
    body = models.TextField(
        blank=True,
        verbose_name=_('Body'),
    )
    author = models.ForeignKey(
        to=get_user_model(),
        related_name='posts',
        related_query_name='post',
        on_delete=models.CASCADE,
        verbose_name=_('Author'),
    )

    objects = PostManager()

    class Meta:
        app_label = 'core'
        verbose_name = _('Post')
        verbose_name_plural = _('Posts')
        db_table = 'post'

    def __str__(self):
        return f'{self.title}'

    def natural_key(self):
        return (self.author.email, self.title)
    natural_key.dependencies = ['auth.user']
```

</assistant_response>

<user_query>
How do I create form from model?
</user_query>

<assistant_response>

```python
from django import forms
from django.contrib.auth import get_user_model
from django.utils.translation import gettext_lazy as _
from myapp.models import Post


class PostForm(forms.ModelForm):
    author = forms.ModelChoiceField(
        queryset=get_user_model().objects.all(),
        label=_('Author'),
        required=True,
    )
    title = forms.CharField(
        label=_('Title'),
        widget=forms.TextInput(attrs={'placeholder': _('Enter title')}),
        max_length=200,
        min_length=5,
        error_messages={
            'min_length': _('Title must be at least 5 characters.'),
            'max_length': _('Title cannot be longer than 200 characters.'),
            'required': _('Title is required.'),
        }
    )
    body = forms.CharField(
        label=_('Body'),
        widget=forms.Textarea(attrs={'placeholder': _('Enter content')}),
        max_length=5000,
        min_length=10,
        error_messages={
            'min_length': _('Body must be at least 10 characters.'),
            'max_length': _('Body cannot be longer than 5000 characters.'),
            'required': _('Body is required.'),
        }
    )

    class Meta:
        model = Post
        fields = ['author', 'title', 'body']
```

</assistant_response>
