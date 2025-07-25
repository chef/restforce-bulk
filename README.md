## Archived 3rd-Party Fork

This repository is an archived copy of a 3rd-party open-source project that Progress Chef contributed to.
It was archived as part of the [Repository Standardization Initiative](https://github.com/chef-boneyard/oss-repo-standardization-2025).
No further contributions are anticipated to be made.

**Original Repository:** [dtmtec/restforce-bulk](https://github.com/dtmtec/restforce-bulk)

---
# Restforce::Bulk

[![build status][1]][2]

[1]: https://travis-ci.org/dtmtec/restforce-bulk.svg
[2]: http://travis-ci.org/dtmtec/restforce-bulk



Client for Salesforce Bulk API.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'restforce-bulk'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install restforce-bulk

## Usage

### Query:

    # Creating a query job and adding a batch, using CSV type
    job   = Restforce::Bulk::Job.create(:query, 'Account', :csv)
    batch = job.add_batch("select Id, Name from Account limit 10")

    # wait for the batch to complete, then refresh data
    batch.refresh
    batch.completed? # => true

    # query batches returns only one result
    result = batch.results.first

    # we can get the contents from the result
    csv = result.content

    # csv is a CSV::Table, now you can process it any way you want

### CRUD operations

    # Creating an upsert job, using XML type (default)
    job = Restforce::Bulk::Job.create(:upsert, 'Account')

    # Adding a batch
    batch = job.add_batch([{ Id: nil, Name: "New Account" }, { Id: 'a0B29000000XGxf', Name: 'Old Account' }])

    # wait for the batch to complete, then refresh data
    batch.refresh
    batch.completed? # => true

    # get the results for each row
    batch.results.each do |result|
      puts result.id      # Id of the result
      puts result.success # row successfully processed
      puts result.error   # error for row
    end

### Binary Attachments

    # Creating an upsert job, either :zip_xml or :zip_csv are accepted
    job = Restforce::Bulk::Job.create(:insert, 'Attachment', :zip_xml)

    # Adding a batch, as an array of file data, with the following keys:
    #   - full_filename: The full path to the file in your filesystem
    #   - filename: The name of the attachment (can have folders in it)
    #   - parent_id: The ID of the parent record for the attachment
    batch = job.add_batch([{ full_filename: "<HOME_DIR>/Pictures/image.jpg", filename: "image.jpg", parent_id: "a0E29000000DzPy" }])

    # wait for the batch to complete, then refresh data
    batch.refresh
    batch.completed? # => true

    # get the results for each row
    batch.results.each do |result|
      puts result.id      # Id of the result
      puts result.success # row successfully processed
      puts result.error   # error for row
    end

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake rspec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/dtmtec/restforce-bulk. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

