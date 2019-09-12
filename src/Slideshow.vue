<template lang='pug'>
.eg-theme-agrume
  #business-logic.eg-slideshow
    slide(enter="fadeIn", leave="fadeOut")
      h1 Dynamic Django models

      h2 Jakub Skałecki @jskalc
      h4.u-text-centered Pycon, 12.09.2019

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 About me
      ul(v-if="step === 1")
        li Currently Technical Team Leader at VideoBeat
        li Co-founder of eSport startup pvpc.eu
        li Backend, DevOps, Frontend, GameDev, Machine Learning...
        li Blogger @ https://rock-it.pl (sometimes)
        li Polyglot programmer, technology enthusiast

      .u-text-centered(v-if="step === 2")
        img.presentation-image(src="./assets/python.png", height="100px")
        h3 5 years

      .u-text-centered(v-if="step === 3")
        img.presentation-image(src="./assets/django.png", height="100px")
        h3 ~4 years

    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Motivation
      h3(v-if="step >= 2 && step <= 3") Static Django Models are great...
      h3(v-if="step === 3") ...but sometimes not flexible enough

      h3(v-if="step === 4") Examples
      ul(v-if="step === 4")
        li Data sets with structure defined by client or market requirements
        li Multi-tenant applications with slightly different clients
        li Content Management Systems

      .u-text-centered(v-if="step === 5")
        h3 Our use case
        p.u-text-centered We're calculating conversion rates using TV advertisements airings data and <strong>custom conversions</strong> defined by clients
        img(src="./assets/tv.png", height="200px")

      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step >= 6")
        blockquote Dynamic models aren't easy. Think twice if it's the good solution for you. In this presentation, I'll try to show you how we've dealt with it.


    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 Other choices
      .u-text-centered(v-if="step === 1", style="height: 50vh;")
        h3 Table with all possible columns
        img(src="./assets/single table.png", style="height: 70%")

      .u-text-centered(v-if="step === 2", style="height: 50vh;")
        h3 Star / Snowflake schema
        img(src="./assets/star_example_trans.png", style="height: 100%")

      .u-text-centered(v-if="step === 3", style="height: 50vh;")
        h3 NoSQL database
        img(src="./assets/cassandra.png", style="height: 40%")
        img(src="./assets/mongo.png", style="height: 40%")

      .u-text-centered(v-if="step === 4", style="height: 50vh;")
        h3 Time-series database
        img(src="./assets/influx.png")


    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 Definition
      .u-text-centered
        eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut')
          blockquote Dynamic Django model can be created, used, updated and removed without changing code
        p(v-if="step > 1") In other words
        eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 2")
          blockquote It allows to store arbitrary data with schema known at the runtime


    slide.u-text-centered(enter="fadeIn", leave="fadeOut", steps="1")
      img.presentation-image.presentation-image--solo(src='./assets/talkischeap.jpg')

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Creating classes in Python
      highlight-code.eg-code-block.code-box(lang="python").
        class RegularClass(RegularBaseClass):
            a = 5

            def normal_function(self):
                return f"I'm a function result! a = {self.a}"

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        >>> obj = RegularClass()
        >>> obj.a
        5
        >>> obj.normal_function()
        "I'm a function result! a = 5"
        >>> isinstance(obj, RegularBaseClass)
        True


    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Our hero - type function
      p(v-if="step === 2") Basic usage
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        >>> type(5)
        <&#8203;class 'int'>

        >>> type('abc')
        <&#8203;class 'str'>

        >>> type(RegularClass())
        <&#8203;class 'RegularClass'>

        >>> type(RegularClass()) == RegularClass
        True

      p(v-if="step === 3") More interesting
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        >>> type(RegularClass)
        <&#8203;class 'type'>

        >>> type(type)
        <&#8203;class 'type'>

      template(v-if="step === 4")
        p With three arguments, returns a new class
        highlight-code.eg-code-block.code-box(lang="python").
          # type(name, bases, dict)
          >>> def func(self):
          ...     return f"I'm a function result! a = {self.a}"
          ...
          >>> DynamicClass = type(
          ...    'DynamicClass',
          ...    (RegularBaseClass,),
          ...    {"a": 5, "normal_function": func}
          ... )
        p This is essentially a dynamic form of the <em>class</em> statement.


      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 5").
        >>> obj = DynamicClass()
        >>> obj.a
        5
        >>> obj.normal_function()
        "I'm a function result! a = 5"
        >>> type(obj) == DynamicClass
        True
        >>> isinstance(obj, RegularBaseClass)
        True

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Django models
      p(v-if="step === 1") Static model
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        class Event(Model):
            timestamp = models.DateTimeField(auto_now_add=True)
            country = models.CharField(max_length=3)
            source = models.CharField(max_length=10)

      p(v-if="step === 2") Same, but dynamic
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        Event = type('Event', (Model,), {
            'timestamp': models.DateTimeField(auto_now_add=True),
            'country': models.CharField(max_length=3),
            'source': models.CharField(max_length=10),
            '__module__': 'myapp.models'  # required by Django
        })


    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 Migrations
      h3 In a regular Django application
      ul
        li(v-if="step > 1") Create the new model
        li(v-if="step > 2") Create migrations
        li(v-if="step > 3") Run migrations

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 Migrations
      h3 With dynamic models
      h1.u-text-centered(v-if="step === 2") ??
      h2.u-text-centered(v-if="step === 3") We have to do it by ourselves!

    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Database API

      .u-text-centered
        img(v-if="step === 1", src="./assets/editor.png")

      p(v-if="step === 2") SchemaEditor
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        from django.db import connection

        with connection.schema_editor() as editor:
          editor.create_model(Event)


      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        >>> Event.objects.create(country='PL', source='mobile')
        <&#8203;Event: Event object (1)>

        >>> e = Event.objects.first()
        >>> e.pk
        1

        >>> e.timestamp
        datetime.datetime(2019, 9, 3, 18, 44, 54, 220157, tzinfo=<&#8203;UTC>)

      h3.u-text-centered(v-if="step === 4") But there is a problem

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 5").
        >>> with connection.schema_editor() as editor:
        ...     editor.create_model(Event)
        ...
        Traceback (most recent call last):
        .
        .
        .
        ProgrammingError: relation "metrics_event" already exists

      h3.u-text-centered(v-if="step === 6") We need to manually check if operation is allowed

    slide(enter="fadeIn", leave="fadeOut", steps="9")
      h2 Our own migrations
      h3.u-text-centered(v-if="step <= 2") Django introspection to the rescue!
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        >>> cursor = connection.cursor()
        >>> introspection.get_table_list(cursor)
        [TableInfo(name='myapp_event', type='t')]

        >>> introspection.get_table_description(cursor, 'myapp_event')
        [
           FieldInfo(name='id', null_ok=False, type_code=23, ...),
           FieldInfo(name='timestamp', null_ok=False, ...),
           ...
        ]

      p(v-if="step === 3") Get existing fields
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        existing_fields = introspection.get_table_description(
          cursor, table_name
        )
        existing_fields_names = {f.name for f in existing_fields}

      p(v-if="step === 4") Compute not created fields
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 4").
        local_fields = model._meta.local_fields
        local_not_existing_fields = {
            f for f
            in local_fields
            if field.column not in existing_fields_names
        }

      p(v-if="step >= 5 && step <= 7") Create missing
      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 5 && step <= 7").
        for field in local_not_existing_fields:
            with connection.schema_editor() as editor:
                editor.add_field(model, field)

      p(v-if="step >= 6  && step <= 7") Remove deleted
      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 6 && step <= 7").
        with connection.schema_editor() as editor:
            editor.remove_field(model, field)

      p(v-if="step === 7") Update changed etc
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 7").
        with connection.schema_editor() as editor:
            editor.alter_field(model, old_field, new_field)

      p(v-if="step === 8") With this knowledge, we can write <em>synchronize</em> func
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 8").
        def synchronize_dynamic_model(model):
            with connection.cursor() as cursor:
                tables = connection.introspection.get_table_list(cursor)
                table_name = model._meta.db_table
                if table_name not in {t.name for t in tables}:
                    with connection.schema_editor() as editor:
                        editor.create_model(model)
                else:
                    # update table logic
                    ...

      p(v-if="step === 9") And use it
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 9").
          attributes = {
            'timestamp': models.DateTimeField(auto_now_add=True),
            '__module__': 'myapp.models'
          }

          Event = type('Event', (Model,), attributes)
          synchronize_dynamic_model(Event)
          Event.objects.create()

          # modify fields and recreate class
          fields['source'] = models.CharField(max_length=10, default='')
          Event = type('Event', (Model,), attributes)
          synchronize_dynamic_model(Event)

          assert Event.objects.first()


    slide.u-text-centered(enter="fadeIn", leave="fadeOut", steps="1")
      img.presentation-image.presentation-image--solo(src='./assets/dynamic.jpg')

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 We need to store definitions somewhere...
      h3(v-if="step === 2") The obvious solution?
      h3(v-if="step === 3") Let's use the database!


    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 Central "model" model
      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 2").
        class DynamicModel(Model):
            name = CharField(max_length=255)

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        class DynamicField(Model):
            model = models.ForeignKey(
                to=DynamicModel,
                related_name="fields"
            )

            name = CharField(max_length=255)
            type = CharField(max_length=20, choices=[
                ('int', 'IntegerField'),
                ('datetime', 'DateTimeField'),
                ('text', 'TextField'),
            ])

    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 Usage example
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        # let's create model
        event_model = DynamicModel(name='event')

        # and fields
        event_time_field = DynamicField(
            name='timestamp',
            type='datetime',
            model=event_model
        )
        event_name_field = DynamicField(
            name='name',
            type='text',
            model=event_model
        )
        event_value_field = DynamicField(
            name='value',
            type='int',
            model=event_model
        )

      p(v-if="step >= 2 && step <= 3") Now we only need matching from type to field...
      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 2 && step <= 3").
        type_to_field = {
            'int': IntegerField(),
            'datetime': DateTimeField(),
            'text': TextField(),
        }

      p(v-if="step === 3") ...construct django fields required by model...
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        fields = {
            f.name: type_to_field[f.type]
            for f in event_model.fields.all()
        }


      p(v-if="step === 4") and we're ready to go!
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 4").
        EventModel = type(event_model.name, (Model,), {
            **fields,
            '__module__': 'myapp.models'
        })

        synchronize_dynamic_model(EventModel)


    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Field options
      h3(v-if="step <= 2") We need to customize our fields
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        class DynamicField(Model):
            ...
            options = JSONField()


        DynamicField(
            name='label'
            type='char',
            options={'max_length': 50}
        )


    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Validation?
      h3(v-if="step === 1") Per-type validators

      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 2 && step <= 3").
        class CharFieldValidator(Form):
            field_cls = CharField
            max_length = IntegerField(default=255)

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        validator = CharFieldValidator(field.options)

        if validator.is_valid():
            data = validator.cleaned_data
            return validator.field_cls(**data)

      highlight-code.eg-code-block.code-box(lang="python", v-if="step >= 4").
        type_to_validator_cls = {
            'int': IntegerFieldValidator,
            'datetime': DateTimeFieldValidator,
            'text': TextFieldValidator,
            'char': CharFieldValidator,
        }

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 5").
        def get_field(type, options):
            validator_cls = type_to_validator_cls[type]
            validator = validator_cls(options)
            if validator.is_valid():
                data = validator.cleaned_data
                return validator.field_cls(**data)
            else:
                raise ValidationError(validator.errors)

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 My favourite shortcut
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        class DynamicField(Model):
            ...

            @property
            def django_field(self):
                return get_field(self.type, self.options)

            # automatically called by django admin and forms
            def clean(self):
                assert self.django_field

      p(v-if="step === 2") Now we can get our field just by calling
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        >>> DynamicField(
        ...   type='char',
        ...   options={'max_length': 100}
        ... ).django_field

        <&#8203;django.forms.fields.CharField object at 0x7fe>

        >>> DynamicField(
        ...   type='int',
        ...   options={'max_length': 'abc'}
        ... ).django_field

        Traceback (most recent call last):
        .
        .
        .
        ValidationError: {'max_length': ['Enter a whole number.']}


      p(v-if="step === 3") Similar shortcut for DynamicModel
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 3").
        class DynamicModel(Model):
          ...

          @property
          def django_model(self):
              return type(self.name, (Model,), {
                  **{f.name: f.field for f in self.fields.all()},
                  '__module__': 'myapp.models'
              })

    slide(enter="fadeIn", leave="fadeOut", steps="1")
      h2 Automatic synchronization
      highlight-code.eg-code-block.code-box(lang="python").
        from django.db.models.signals import post_save

        @post_save(sender=DynamicField)
        def synchronize(field_instance, **kwargs):
            synchronize_dynamic_model(field_instance.model.django_model)

    slide(enter="fadeIn", leave="fadeOut", steps="1")
      h2 Final usage
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        # just create model and fields
        event_model = DynamicModel(name='event')
        DynamicField.objects.create('timestamp', 'datetime', event_model)
        DynamicField.objects.create('name', 'text', event_model)

        # and it's ready to use
        EventModel = event_model.django_model
        EventModel.objects.all()

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Bonus: Usage with DRF
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        class DynamicModelViewSet(ModelViewSet):

          def get_model(self):
            return DynamicModel.objects.get(pk=self.kwargs["model_pk"])

          def get_queryset(self):
            dynamic_model = self.get_model()
            return dynamic_model.django_model.objects.all()

          def get_serializer_cls(self):
            class Meta:
              fields = '__all__'
              model = model_cls

            return type(
              "DynamicModelSerializer",
              (ModelSerializer,),
              {"Meta": Meta}
            )

      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        urlpatterns = [
          path("model/<&#8203;int:model_pk>/", DynamicModelViewSet.as_view())
        ]
    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Warning
      p(v-if="step === 1") Related name conflicts
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        type(DynamicModel, (Model,), {
            'field': ForeignKey(
                to=StaticModel,
                related_name="dynamic_model"
            )
        }

        type(OtherDynamicModel, (Model,), {
            'field': ForeignKey(
                to=StaticModel,
                related_name="dynamic_model"
            )
        }


      p(v-if="step === 2") Solution
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        type(DynamicModel, (Model,), {
            'field': ForeignKey(
                to=StaticModel,
                related_name="+"  # no reverse relation
            )
        }

        type(OtherDynamicModel, (Model,), {
            'field': ForeignKey(
                to=StaticModel,
                related_name="+"  # no reverse relation
            )
        }



    slide(enter="fadeIn", leave="fadeOut", steps="1")
      h2 Pros
      ul
        li Dynamic, runtime-defined shape of the database
        li Lower infrastructure complexity (single database for dynamically-shaped and static data)
        li Fast writes / reads (compared to Star schema)

    slide(enter="fadeIn", leave="fadeOut", steps="1")
      h2 Cons
      ul
        li Higher code complexity (initial bootstrapping and future usage)
        li Requires some Django "hacks"
        li Hard time with Foreign Keys

    slide(enter="fadeIn", leave="fadeOut", steps="1")
      h1 Thank you!
      h2 Any questions?

</template>

<script>
import eagle from 'eagle.js'
import CinemaPlayer from './AsciinemaPlayer'

export default {
  mixins: [ eagle.slideshow ],
  components: {CinemaPlayer},
  data () {
    return {
      shortVersion: true
    }
  }
}
</script>

<style lang='scss'>
  @import url(https://fonts.googleapis.com/css?family=Oxygen);
  #business-logic {
    //background-image: url("~eagle.js/dist/themes/assets/crossword.png");

    &.eg-slideshow::after {
      content: "@jskalc";
      padding-right: 45px;
      padding-top: 3px;
      position: absolute;
      font-size: 0.8em;
      right: 20px;
      bottom: 20px;
      background: url(./assets/twitter.png) no-repeat right;
      background-size: contain;
    }

    &.eg-slideshow::before {
      content: "Valian";
      position: absolute;
      padding-left: 45px;
      padding-top: 5px;
      font-size: 0.8em;
      left: 20px;
      bottom: 20px;
      color: #0e3d54;
      background: url(https://image.flaticon.com/icons/svg/25/25231.svg) no-repeat;
      background-size: contain;
    }

    h1, h2, h3, h4, div, blockquote, p, ul, li {
      font-family: "Oxygen", sans-serif;
      color: #184555;
    }

    h1 { font-size: 3.2em; color: #184555; }
    h2 { font-size: 2.1em; }
    h3 { font-size: 1.5em; text-transform: none;}
    h4 { font-size: 1.2em; }
    blockquote {
      font-style: italic;
    }
    ul li {
      margin-bottom: 0.3em;
    }
    .eg-code-block {
      font-size: 0.6em;
    }

    .presentation-image {
      &--solo {
        margin: 15% auto;
      }
    }
  }
  .u-text-centered {
    text-align: center;
  }
</style>
