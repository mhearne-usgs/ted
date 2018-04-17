tweet_fetcher_readme.md

Introduction
------------

Tweet_fetcher is an application which establishes a tweet stream using the Twitter API, filters out tweets containing earthquake keywords, and adds these tweets to a Postgres table.

Installation and Dependencies
-----------------------------

The ted conda environment must be activated for whichever user is running PDL. The PDL runs this code when a new event or event update (origin product) is received. To activate this environment for that user, type:
    source activate ted
If the environment has not been created yet or does not exist, type:
    ./install.sh 

The following tables must exist in Postgres:
- keyword
- message

If these tables have not been created, first activate the PostgreSQL Test_DB from the terminal by typing:

    psql -D ted -u ted -p 5433

And entering the same password that is used for the TED Dev database.
Create the keyword table by typing:

    create sequence keyword_id_seq;
    create table keyword(
        id integer not null primary key default nextval('keyword_id_seq'), 
        title character varying(60)
    );
    insert into keyword(title)
    values ('earthquake','sismo','quake','temblor','terremoto','gempa',
            'lindol','tremblement','erdbeben','deprem','σεισμός','seismós',
            'séisme','zemljotres','potres','terremot','jordskjelv','cutremur',
            'aardbeving','地震','भूकंप','زلزال','tremor','지진');

Create the message table by typing:

    create sequence message_id_seq;
    create table message(
        id bigint not null primary key default nextval('message_id_seq'),
        date_created timestamp without time zone not null,
        lang character varying(5),
        location_lat numeric(11,7),
        location_lon numeric(11,7),
        location_source character varying(10),
        location_string character varying(100),
        media_display_url character varying(255),
        media_type character varying(10),
        message_date timestamp without time zone not null,
        message_id bigint,
        message_source character varying(20) not null,
        message_text character varying(290),
        time_zone character varying(80),
        utc_offset bigint,
        word_count integer
    );

Tweet_fetcher has been designed to run in Python 3.

Running tweet_fetcher.py
------------------------

Run tweet_fetcher as a background process by typing:
    python tweet_fetcher &