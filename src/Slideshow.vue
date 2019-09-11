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
        li Blogger (sometimes)
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


    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Other choices
      ul
        li(v-if="step > 1") Single table with all possible columns
        li(v-if="step > 2") Star / Snowflake schema
        li(v-if="step > 3") NoSQL database
        li(v-if="step > 4") Time-series database


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
            '__module__': 'myapp.models'
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

    slide(enter="fadeIn", leave="fadeOut", steps="2")
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

      p(v-if="step === 2") Now we only need matching from type to field...
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        type_to_field = {
            'int': IntegerField(),
            'datetime': DateTimeField(),
            'text': TextField(),
        }

      p(v-if="step === 2") And our dynamic model is ready!
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 2").
        fields = {
            f.name: type_to_field[f.type]
            for f in event_model.fields.all()
        }

        EventModel = type(event_model.name, (Model,), {
            **fields,
            '__module__': 'myapp.models'
        })

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
        def get_field(type, config):
            validator_cls = type_to_validator_cls[type]
            validator = validator_cls(field.options)
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
      h2 Final usage
      highlight-code.eg-code-block.code-box(lang="python", v-if="step === 1").
        EventModel = DynamicModel.objects.get(name='event').django_model
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


    slide(enter="fadeIn", leave="fadeOut")
      .u-text-centered
        <!-- img.presentation-image&#45;&#45;solo(src="./assets/docker.svg")-->

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Docker is a solution
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step === 2")
          h1 But what is the problem?

    slide(enter="fadeIn", leave="fadeOut", steps="4")
      br
      eg-transition(v-if="step > 1", enter='bounceInLeft', leave='fadeOut')
        h3 Development environment setup & updates
      eg-transition(v-if="step > 2", enter='bounceInLeft', leave='fadeOut')
        h3 Predictable, repeatable deployment
      eg-transition(v-if="step > 3", enter='bounceInLeft', leave='fadeOut')
        h3 Server resource utilization

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 Let's focus on the first one
      h2(v-if="step > 1") Development setup
      h3(v-if="step === 3") How are you dealing with it?

    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Development setup
      p Previous approaches (my experience)
      ul(v-if="step > 1 && step < 5")
        li manual configuration using never written, "tribal knowledge" how to setup things
        li(v-if="step > 2") no separation between projects - different version of database anyone?
        li(v-if="step > 3") Virtual machines are a partial solution, but adds overhead
      .u-text-centered
        <!--img.presentation-image(src="./assets/ha-ha-works-on-my-machine.jpg", v-if="step > 4")-->

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 How Docker can help you?
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step === 2")
        h1 What is docker, after all?

    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 My docker definition
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 1")
        blockquote Docker is a framework to predictably create and run isolated, lightweight containers

      h2(v-if="step > 2") And container?
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 3")
        blockquote Container is a standard unit of software that packages up code and all its dependencies

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Hello world
      highlight-code.eg-code-block.code-box(lang="bash", v-if="step === 1").
        # -i and -t are needed to connect input / output to the container
        docker container run -it python

        # after a while, it will show up
        Python 3.7.1 (default, Oct 24 2018, 22:35:30)
        [GCC 6.3.0 20170516] on linux
        Type "help", "copyright", "credits" or "license" for information.
        >>>|

      cinema-player.u-text-centered(id="902zfwSgmDLLk58JCEoHqZIvr", size="big", v-if="step === 2")

    slide(enter="fadeIn", leave="fadeOut")
      .u-text-centered
        eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut')
          <!-- img.presentation-image&#45;&#45;solo(src="./assets/magic.gif", width="80%")-->


    slide(enter="fadeIn", leave="fadeOut", steps="5")
      h2 Hello world explanation
      ul
        li(v-if="step > 1") First, docker checks if image called python:alpine exists locally
        li(v-if="step > 2") Since it doesn't, it checks if image exists in remote registry
        li(v-if="step > 3") Yes, <a href="https://hub.docker.com/_/python/">it exists</a>, so docker pulls it
        li(v-if="step > 4") And starts a container from python image with a command defined in the image <pre>python</pre>

    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 Docker hub
      p Biggest collection of publicly-available, actively maintained images
      ul(v-if="step > 1")
        li postgres? Sure!
        li mysql? Of course!
        li redis, rabbitmq, nginx? Also!
        li your favourite language? I bet it's there!
        li(v-if="step > 2") literally <strong>anything</strong>
      p(v-if="step > 3") You can create and upload your own

    slide(enter="fadeIn", leave="fadeOut", steps="4")
      h2 Docker image
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 1")
        blockquote A snapshot of a container, used to create other containers
      h3(v-if="step > 2") OOP analogy
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 3")
        blockquote Image is a class, container is an object

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Basic workflow demo

      highlight-code.eg-code-block.code-box(lang="bash", v-if="step === 1").
        # download the newest version of the image
        docker image pull postgres

        # run in background, exposing port 5432 to host
        docker run -d --name database -p 5432:5432 postgres

        # display running containers
        docker ps

        # start / stop / remove container
        docker stop / start / rm database

        # remove image
        docker image rm postgres
      cinema-player.u-text-centered(id="gULNnYKTNR7efjbe7DST3SPqn", size="big", v-if="step === 2")

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Dockerfile
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 1")
        blockquote A step-by-step recipe how to create an image

    slide(enter="fadeIn", leave="fadeOut")
      h2 Dockerfile example
      highlight-code.eg-code-block.code-box(lang="dockerfile").
        FROM python:3.7

        WORKDIR /srv

        # install system packages
        RUN apt-get update \
         && apt-get install -y default-libmysqlclient-dev \
         && rm -rf /var/lib/apt/lists/*

        # install python packages
        COPY requirements.txt .
        RUN pip install -r requirements.txt

        # copy rest of the code
        COPY . .

    slide(enter="fadeIn", leave="fadeOut")
      h2 Dockerfile usage
      cinema-player.u-text-centered(id="AfV3PhnWJhePfnftUaI7JXQCU", size="big")

    slide(enter="fadeIn", leave="fadeOut", steps="3")
      h2 Docker Compose
      eg-transition.u-text-centered(enter='bounceInLeft', leave='fadeOut', v-if="step > 1")
        blockquote Tool for managing multiple containers configuration
      h3(v-if="step > 2") Very useful for development


    slide(enter="fadeIn", leave="fadeOut")
      h2 "Real world" example
      highlight-code.eg-code-block.code-box(v-if="step === 1", lang="yaml").
        # docker-compose.yaml
        version: '2'
        services:
          database:
            image: mysql:5.7
            environment:
              MYSQL_ROOT_PASSWORD: 'secret'

          redis:
            image: redis

          app:
            build: .
            command: python manage.py runserver 0.0.0.0:8000
            volumes:
              - ./src/django:/srv
            environment:
              DJANGO_SETTINGS_MODULE: test.settings.development
            ports:
              - 8000:8000


    slide(enter="fadeIn", leave="fadeOut")
      h2 Demo from one of our projects
      cinema-player.u-text-centered(id="afEYDA72DoFUXeWZA4ZS11K55", size="big")

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 The best part?
      h1(v-if="step === 2") It's the same for every project!

    slide(enter="fadeIn", leave="fadeOut", steps="2")
      h2 Next steps - production environment benefits
      ul
        li Predictable deployment - exact same environment across multiple servers
        li Ops doesn't have to worry about dependencies, they just "juggle" containers.
        li Lower costs - you can put multiple isolated containers into one server

    slide(enter="fadeIn", leave="fadeOut")
      h2 Comparision vs VM
      .u-text-centered
        <!-- img.presentation-image(src="./assets/docker_vs_vm.jpeg")-->

    slide(enter="fadeIn", leave="fadeOut")
      h2 Thank you!
      h3 Questions?

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
      background: url(https://logos-download.com/wp-content/uploads/2016/02/Twitter_logo_bird_transparent_png.png) no-repeat right;
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
