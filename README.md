travis_after_all
================

[![Build Status](https://travis-ci.org/dmakhno/travis_after_all.png?branch=master)](https://travis-ci.org/dmakhno/travis_after_all)

This is Travis CI helper to run particular work only once in matrix.

This is workaround for: https://github.com/travis-ci/travis-ci/issues/929

The main goal of this script to have a single publish when a build has several jobs. Currently the first job is a leader, meaning a node that will do the publishing.

An example .travis.yml shows how to ensure that `all_succeeded` or `all_failed`:

```yaml
#...
script:
  - curl -Lo travis_after_all.py https://raw.github.com/dmakhno/travis_after_all/master/travis_after_all.py
after_success:
  - python travis_after_all.py
  - export $(cat .to_export_back)
  - |
      if [ "$BUILD_LEADER" = "YES" ]; then
        if [ "$BUILD_AGGREGATE_STATUS" = "others_succeeded" ]; then
          echo "All jobs succeeded! PUBLISHING..."
        else
          echo "Some jobs failed"
        fi
      fi
after_failure:
  - python travis_after_all.py
  - export $(cat .to_export_back)
  - |
      if [ "$BUILD_LEADER" = "YES" ]; then
        if [ "$BUILD_AGGREGATE_STATUS" = "others_failed" ]; then
          echo "All jobs failed"
        else
          echo "Some jobs failed"
        fi
      fi
after_script:
  - echo leader=$BUILD_LEADER status=$BUILD_AGGREGATE_STATUS
```

Limitations/Todo
----------------

- If other jobs will start late, can be global build timeout (think of passing leader role to others, for example who started last is the leader).
- More flexible leader definition (in matrix not all jobs can publish).
- Have several leaders, according to flavours (e.g. if in matrix slices responsible for platform).
