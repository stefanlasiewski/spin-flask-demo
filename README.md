
# Containers

On your laptop set up like, for instance:

    docker pull mongo:latest
    docker run --name db \
        -e MONGO_INITDB_ROOT_USERNAME=... \
        -e MONGO_INITDB_ROOT_PASSWORD=... \
        -d mongo

    cd app
    docker build -t app:latest .
    docker run --name app \
        -e MONGO_INITDB_ROOT_USERNAME=... \
        -e MONGO_INITDB_ROOT_PASSWORD=... \
        -p 8000:8000 \
        --link db:db \
        -d app

    cd ../web
    docker pull nginx:latest
    docker build -t web:latest .
    docker run --name web \
        -v $(pwd)/images:/srv:ro \
        -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
        -p 80:80 \
        --link app:app \
        -d web

# Data

Data set comes from the Broad Bioimage Benchmark Collection:
Annotated biological image sets for testing and validation.
Drosophila Kc167 cells:
<https://data.broadinstitute.org/bbbc/BBBC002/>

Five different samples of Drosophila melanogaster Kc167 cells were stained with Hoechst 33342, a DNA stain.
The last sample (labeled nodsRNA) is of wild-type cells.
Each of the other four samples (labeled 48, 340, Anillin, and mad2) has a different gene knocked down by RNAi.
The sample preparation is described in more detail by Carpenter et al. (Genome Biology, 2006).

## Images

There are 10 fields of view of each sample, for a total of 50 fields of view.
The images were acquired on a Zeiss Axiovert 200M microscope.
The images provided here are a single channel, DNA.
The image size is 512 x 512 pixels.
The images are provided as 8-bit TIFF files.

<https://data.broadinstitute.org/bbbc/BBBC002/BBBC002_v1_images.zip>

The images were converted to PNG from TIFF.

## Ground Truth

A tab-delimited text file contains the number of cells in each image, as determined by two different human counters.
To compare an algorithm's results to these, first compute for each sample the algorithm's mean cell count over the 10 images of the sample.
Next, calculate the absolute difference between this mean and the average of the humans' counts for the sample, then divide by the latter to obtain the deviation from ground truth (in percent).
The mean of these values over all 5 samples is the final result.

Note: The two human observers vary by 16% for this image set.

<https://data.broadinstitute.org/bbbc/BBBC002/BBBC002_v1_counts.txt>

We just incorporate this into the app set up.
At start up the MongoDB is seeded with the metadata to make the example more interesting.



