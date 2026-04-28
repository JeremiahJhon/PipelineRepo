Questions
o How did you test your pipelines?
    - I tested the pipelines using a local installation of Jenkins on Windows 11.
    - Confirmed outputs: doc.zip (generated documentation), output.csv (parsed warnings in TaskC)
o How did you test repoC python?
    - I tested the Python parser independently before integrating it into the pipeline.
    - Created sample warnings.log files with:
        Valid Doxygen warning lines
        Invalid/non-standard lines (to verify they are ignored)
        Edge cases (empty file, malformed entries)
    - Ran the script locally:
        python parser.py warnings.log
     - Verified that:
        Output CSV is correctly generated
        Only valid warning lines are parsed
        Columns (Line, File, Message) are accurate
o RepoA-doc contains binaries
    ▪ What is the advantage to use LFS?
        - It enables efficient storage which reduces the repository size, thus improves performance
    ▪ <https://git-lfs.github.com/>
o How to adjust this repository to support LFS?
    ▪ provide links
    ▪ You might find the git way
    ▪ Are there other (easier) alternatives?
        - Yes—depending on the use case, there are simpler or more scalable alternatives like Cloud Storage: Azure storage, AWS S3, GCP Bucket