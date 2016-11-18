# Snap Processor Plugin - Statistics
Snap plugin intended to process data and return statistics over a sliding window.

1. [Getting Started](#getting-started)
  * [System Requirements](#system-requirements)
  * [Installation](#installation)
  * [Configuration and Usage](configuration-and-usage)
2. [Documentation](#documentation)
  * [Examples](#examples)
  * [Roadmap](#roadmap)
3. [Community Support](#community-support)
4. [Contributing](#contributing)
5. [License](#license)
6. [Acknowledgements](#acknowledgements)


## Getting Started
### System Requirements
* Linux/amd64
* OS X

### Installation
#### Download plugin binary: 

You can get the pre-built binaries for your OS and architecture at Snap's [Github Releases](https://github.com/intelsdi-x/snap/releases) page.

#### To build the plugin binary:
Fork https://github.com/intelsdi-x/snap-plugin-processor-statistics

Clone repo into `$GOPATH/src/github/intelsdi-x/`:
```
$ git clone https://github.com/<yourGithubID>/snap-plugin-processor-statistics
```
Build the plugin by running make in repo:
```
$ make
```
This builds the plugin in `./build`

Unit testing:
```
$ make test-small
```

### Configuration and Usage
* Set up the [snap framework](https://github.com/intelsdi-x/snap/blob/master/README.md#getting-started)

## Documentation
To illustrate the sliding window concept, let's say we have a sliding window of 10 over 100 points long, if the sliding factor is 1 and we want to calculate the statistic "average", it will take the average from 0-10, 1-11, 2-12, 3-13 and so on. That takes the 100 data points and averages to 99 points.
Sliding window can be used to smooth out a waveform thereby reducing noise. It is very useful in handling noisey data and also in live streaming of data.
In regular windowing, it takes the average of data points 0-10, then 11-20, then 21-30 and so on. So there will be 10 points instead of 100.
So in regular windowing, we have two variables, the size of the dataset, and the size of the window. Since sliding windows "overlap" the previous and following windows, we have the total size, window size and sliding factor.  		

The default values of sliding factor is 1 and the interval is 1s. Sliding window length default is 100.		
Here's an illustration of the sliding window length concept and also the list of statistics calculated by the processor statistics plugin along with their descriptions-
[Sliding Window.pdf](https://github.com/intelsdi-x/snap-plugin-processor-statistics/files/599298/Sliding.Window.pdf)
		

### Examples
Creating a task manifest file. 
```
{
    "version": 1,
    "schedule": {
        "type": "simple",
        "interval": "1s"
    },
    "max-failures": 2,
    "workflow": {
        "collect": {
            "metrics": {
                "/intel/mock/foo": {},
                "/intel/mock/bar": {},
            },
            "config": {
                "/intel/mock": {
                    "user": "root",
                    "password": "secret"
                }
            },
            "process": [
                {
                    "plugin_name": "statistics",
		    "config":
    			{
	    		"slidingWindowLength": 5,
                   "slidingFactor": 2,
			},		
                    "publish": [
                        {
                            "plugin_name": "file",
                            "config": {
                                "file": "/tmp/published"
                            }
                        }
                    ]
                }
            ]
        }
    }
}
```

### Roadmap
There isn't a current roadmap for this plugin, but it is in active development. As we launch this plugin, we do not have any outstanding requirements for the next release.

If you have a feature request, please add it as an [issue](https://github.com/intelsdi-x/snap-plugin-processor-statistics/issues) and feel free to then submit a [pull request](https://github.com/intelsdi-x/snap-plugin-processor-statistics/pulls).

## Community Support
This repository is one of **many** plugins in **Snap**, the open telemetry framework. See the full project at http://github.com/intelsdi-x/snap. To reach out to other users, head to the [main framework](https://github.com/intelsdi-x/snap#community-support).

## Contributing
We love contributions!

There's more than one way to give back, from examples to blogs to code updates. See our recommended process in [CONTRIBUTING.md](CONTRIBUTING.md).

And **thank you!** Your contribution, through code and participation, is incredibly important to us.

## License
[Snap](http://github.com:intelsdi-x/snap), along with this plugin, is an Open Source software released under the Apache 2.0 [License](LICENSE).

## Acknowledgements

* Authors: [Rashmi Gottipati](https://github.com/rashmigottipati),
           [Balaji Subramaniam](https://github.com/balajismaniam)

And **thank you!** Your participation and contribution through code is important to us.

