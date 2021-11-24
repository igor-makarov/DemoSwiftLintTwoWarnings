require 'yaml'

project.name = 'DemoSwiftLintTwoWarnings'

application_for :ios, '13.0' do |target|
  target.name = 'DemoSwiftLintTwoWarnings'
  target.all_configurations.each do |config|
    config.product_bundle_identifier = 'com.igor.demoswiftlinttwowarnings'
  end

  swiftlint_file = '${BUILT_PRODUCTS_DIR}/SwiftLint-touched.txt'
  swiftlint_phase_script = <<-SWIFTLINT
  trap 'exit 0;' INT
  "${PODS_ROOT}"/SwiftLint/swiftlint && touch #{swiftlint_file}
  SWIFTLINT
  target.pre_shell_script_build_phase('SwiftLint', swiftlint_phase_script) do |phase|
    config_excludes = YAML.load_file('.swiftlint.yml')['excluded']
                          .map { |filename| filename.end_with?('.swift') ? [filename] : Dir["#{filename}/**/*.swift"] }
                          .flatten
    file_list = (Dir['**/*.swift'] - config_excludes).sort
    phase.input_paths = file_list.map { |f| "${SRCROOT}/#{f}" }
    phase.output_paths = [swiftlint_file]
  end
end

project.after_save do
  system "rm -rf \"#{project.name}.xcodeproj/xcshareddata/xcschemes\""
end