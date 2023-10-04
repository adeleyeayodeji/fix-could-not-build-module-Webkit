* Paste at the end of your PODS config *

```pod
post_integrate do |installer|
  compiler_flags_key = 'COMPILER_FLAGS'
  project_path = 'Pods/Pods.xcodeproj'

  project = Xcodeproj::Project.open(project_path)
  project.targets.each do |target|
    target.build_phases.each do |build_phase|
      if build_phase.is_a?(Xcodeproj::Project::Object::PBXSourcesBuildPhase)
        build_phase.files.each do |file|
          if !file.settings.nil? && file.settings.key?(compiler_flags_key)
            compiler_flags = file.settings[compiler_flags_key]
            file.settings[compiler_flags_key] = compiler_flags.gsub(/-DOS_OBJECT_USE_OBJC=0\s*/, '')
          end
        end
      end
    end
  end
  project.save()
end
```
